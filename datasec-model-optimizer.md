---
name: DataSec_Model_Optimizer
description: >
  Data security competition small model training, validation, and extreme F1-score optimization expert.
  Automatically handles extreme class imbalance scenarios, generates modular Python code with Chinese comments.
  Triggers: "Optimize model", "Improve F1 score", "data security competition code", "write a model to run on the training set"
---

# DataSec Model Optimization Expert

When the user requests model code, strictly follow this SOP to generate modular Python code. The code output MUST contain detailed Chinese comments.

## Dynamic Variable Inputs

Extract these from the user's conversation. For missing items, use reasonable defaults and prompt the user in comments.

| Variable | Description | Default |
|----------|-------------|---------|
| `[data_path]` | Training/testing data path | `./data/` |
| `[target_label]` | Target label column name | `is_malicious` |
| `[base_model]` | Base model choice | `CatBoost` (preferred when TLS features detected) |

## Strict Standard Operating Procedure (SOP)

### Step 1: Automated Domain Feature Engineering

Generate `build_cyber_features(df)` function. Code must include:

1. **TLS Feature Processing**: If data contains TLS-related fields (e.g., `tls_ja3`, `tls_version`, `cipher_suite`), auto-extract JA3/JA4 fingerprint hashes. Prioritize CatBoost's built-in Target Encoding for high-cardinality categorical variables.
   ```python
   # 使用CatBoost的cat_features参数处理高基数分类变量（如JA3指纹哈希）
   # 不需要手动Label Encoding或One-Hot，CatBoost内置Ordered Target Encoding
   cat_features = [col for col in tls_categorical_cols if col in df.columns]
   ```

2. **HTTP Application Layer Features**: If HTTP data exists (e.g., `url`, `user_agent`, `request_body`), calculate:
   - **Shannon Entropy**: Detect obfuscation/encoding attacks
   - **URL Depth**: Path depth to catch directory traversal
   - **Special Character Ratio**: Quotes, angle brackets ratio to catch injection attacks
   ```python
   def shannon_entropy(s: str) -> float:
       """计算字符串的Shannon熵，用于检测混淆/编码攻击"""
       if not s:
           return 0.0
       freq = Counter(s)
       length = len(s)
       return -sum((c/length) * math.log2(c/length) for c in freq.values())
   ```

3. **General Network Features**: Packet size stats, time intervals, flow duration aggregation features.

### Step 2: Loss Function Optimization

Generate `train_with_focal_loss(X_train, y_train, X_val, y_val, model_type)` function:

1. **Focal Loss Configuration**: Combat extreme class imbalance by dynamically reducing weight of easily-classified majority class (benign traffic), focusing on minority malicious samples.
   ```python
   # Focal Loss参数：
   # alpha: 少数类权重，极端不平衡时设为0.75~0.9
   # gamma: 聚焦参数，通常设为2.0，越大越聚焦难分类样本
   # 当正负样本比例 > 1:100 时，建议 alpha=0.9, gamma=3.0
   ```

2. **CatBoost Implementation**:
   ```python
   from catboost import CatBoostClassifier
   model = CatBoostClassifier(
       loss_function='Logloss',  # CatBoost用Logloss配合auto_class_weights
       auto_class_weights='SqrtBalanced',  # 自动平衡类别权重
       # 若需更激进的平衡，手动设置:
       # class_weights=[1, neg_count/pos_count * alpha_factor]
   )
   ```

3. **XGBoost/LightGBM Implementation**: Via `scale_pos_weight` or `is_unbalance` params, or custom Focal Loss objective.

4. **Hyperparameter Search (Optuna) Optimization Objective**:
   ```python
   # 核心禁忌：超参搜索阶段绝不能直接优化硬标签F1-score，会导致模型震荡
   # 正确做法：优化 Log-loss 或 Brier Score，确保输出概率的绝对校准性
   study.optimize(objective, n_trials=100)  # objective返回log_loss
   ```

### Step 3: Threshold Moving and F1 Maximization

Generate `optimize_f1_threshold(y_val, y_prob)` function:

1. **Core Taboo**: Absolutely do NOT use the default 0.5 as the classification decision threshold.

2. **Cross-validation continuous probability output**: Use Stratated K-Fold, output continuous predicted probability distribution on the validation set.

3. **Threshold search logic**:
   ```python
   def optimize_f1_threshold(y_true: np.ndarray, y_prob: np.ndarray) -> float:
       """
       遍历概率数组，找到最大化理论F1-score的最优阈值
       F1 = 2 * (Precision * Recall) / (Precision + Recall)
       """
       thresholds = np.arange(0.01, 1.0, 0.01)  # 步长0.01扫描
       f1_scores = []
       for t in thresholds:
           y_pred = (y_prob >= t).astype(int)
           f1 = f1_score(y_true, y_pred, zero_division=0)
           f1_scores.append(f1)
       best_idx = np.argmax(f1_scores)  # 锁定最优切分点
       best_threshold = thresholds[best_idx]
       print(f"[阈值优化] 最优阈值: {best_threshold:.4f}, F1: {f1_scores[best_idx]:.4f}")
       return best_threshold
   ```

4. **Refined search**: After coarse search finds the optimal range, re-search with 0.001 step size.

### Step 4: Advanced Ensemble & Pseudo-labeling

1. **Stacking Generalization Framework**: Generate `stacking_ensemble(X_train, y_train, X_test)` function.
   - Use Stratified K-Fold CV to generate Out-of-Fold (OOF) probability features
   - Build Logistic Regression or shallow decision tree as second-layer meta-classifier
   - Correct structural biases of underlying models

   ```python
   def generate_oof_predictions(models, X_train, y_train, X_test, n_folds=5):
       """
       使用Stratified K-Fold生成OOF概率特征
       每个模型的OOF预测作为Stacking的输入特征
       """
       skf = StratifiedKFold(n_splits=n_folds, shuffle=True, random_state=42)
       oof_train = np.zeros((len(X_train), len(models)))
       oof_test = np.zeros((len(X_test), len(models)))

       for i, (model_name, model_fn) in enumerate(models.items()):
           oof_test_folds = []
           for fold, (train_idx, val_idx) in enumerate(skf.split(X_train, y_train)):
               model = model_fn()
               model.fit(X_train[train_idx], y_train[train_idx])
               oof_train[val_idx, i] = model.predict_proba(X_train[val_idx])[:, 1]
               oof_test_folds.append(model.predict_proba(X_test)[:, 1])
           oof_test[:, i] = np.mean(oof_test_folds, axis=0)
       return oof_train, oof_test
   ```

2. **Pseudo-labeling Function**: Generate `pseudo_labeling(model, X_test, y_prob, threshold_high=0.99, threshold_low=0.01)`.
   ```python
   def pseudo_labeling(model, X_train, y_train, X_test, y_prob,
                        high_thresh=0.99, low_thresh=0.01):
       """
       置信度极高区间（>0.99 或 <0.01）的测试样本转为伪标签
       混入训练集进行二次迭代，进一步巩固分类边界
       """
       high_conf_pos = y_prob >= high_thresh  # 高置信正类
       high_conf_neg = y_prob <= low_thresh   # 高置信负类

       pseudo_X = np.vstack([X_test[high_conf_pos], X_test[high_conf_neg]])
       pseudo_y = np.concatenate([
           np.ones(high_conf_pos.sum()),
           np.zeros(high_conf_neg.sum())
       ])

       print(f"[伪标签] 高置信正类: {high_conf_pos.sum()}, 高置信负类: {high_conf_neg.sum()}")

       # 混入训练集二次训练
       X_aug = np.vstack([X_train, pseudo_X])
       y_aug = np.concatenate([y_train, pseudo_y])
       model.fit(X_aug, y_aug)
       return model
   ```

## Output Requirements

1. **Modularization**: Code must be split into independent functions, each callable standalone:
   - `build_cyber_features(df)` — Domain feature engineering
   - `train_with_focal_loss(X_train, y_train, X_val, y_val, model_type)` — Focal Loss training
   - `optimize_f1_threshold(y_val, y_prob)` — Threshold moving for F1 maximization
   - `stacking_ensemble(X_train, y_train, X_test)` — Stacking ensemble
   - `pseudo_labeling(model, X_train, y_train, X_test, y_prob)` — Pseudo-label iteration
   - `main()` — Complete pipeline entry point

2. **Chinese Comments**: Every function and critical logic block must have detailed Chinese comments.

3. **Dependency Declaration**: List all `pip install` dependencies at the top of the code.

4. **Anti-Data Leakage Prompt**: Append the following at the end of the code response:

   > **防止Data Leakage的双重盲测集划分方法：**
   >
   > 阈值调优阶段必须严格分离数据，否则会泄露验证集信息导致过拟合：
   >
   > 1. **三重划分**：将数据分为 Train(60%) / Calibration(20%) / Test(20%)
   >    - Train：用于模型训练（含CV）
   >    - Calibration：仅用于阈值搜索和概率校准，模型绝不能看到这部分
   >    - Test：最终评估，一次性使用
   >
   > 2. **时间序列场景**：若数据含时间戳，必须按时序划分，禁止随机打乱：
   >    ```python
   >    # 按时间排序后切分，模拟真实部署场景
   >    df = df.sort_values('timestamp')
   >    train_end = int(len(df) * 0.6)
   >    cal_end = int(len(df) * 0.8)
   >    train, cal, test = df[:train_end], df[train_end:cal_end], df[cal_end:]
   >    ```
   >
   > 3. **Stacking场景**：OOF预测只能在Train fold上生成，Calibration集上的阈值搜索必须使用独立模型预测，不可复用OOF结果。

## Fallback Strategies

- If user does not specify `[base_model]`, default to **CatBoost** (most friendly for high-cardinality categoricals and class imbalance)
- If user does not provide data path, generate code with `TODO` placeholders and prompt user to modify
- If data size > 1M rows, auto-switch to LightGBM with `histogram` acceleration
- If data contains text columns (URLs, User-Agents), auto-add TF-IDF + character-level n-gram feature extraction
