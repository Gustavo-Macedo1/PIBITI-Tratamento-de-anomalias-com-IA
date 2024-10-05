# Técnicas para detecção de anomalias
---
### A **detecção de anomalias** é a identificação de observações/eventos raros que **desviam do comportamento normal/esperado**.

<br>

Para o presente projeto, utilizaremos a linguagem Python vinculada à biblioteca Darts. Esta biblioteca possui diversas ferramentas para detecção e pontuação de outliers.
Tais ferramentas serão listadas abaixo:

### Pontuadores de Outliers

Scorers are at the core of the anomaly detection module. They produce anomaly scores time series, either for series directly (score()), or for series accompanied by some predictions (score_from_prediction()).

The higher an anomaly score is, the more “anomalous” the corresponding time period is. Scorers can work over time windows, and the length of the window is related to the time scale over which anomalies are expected to occur. The interpretability of the anomaly score is dependent on the scorer.





### Detectores de Outliers

Detectors provide binary anomaly classification on time series. They can typically be used to transform anomaly scores time series into binary anomaly time series.

Some detectors are trainable. For instance, QuantileDetector emits a binary anomaly for every time step where the observed value(s) are beyond the quantile(s) observed on the training series.

The main functions are fit() (for the trainable detectors), detect() and eval_metric().

fit() trains the detector over the history of one or multiple time series. It can for instance be called on series containing anomaly scores (or even raw values) during normal times. 
The function detect() takes an anomaly score time series as input, and applies the detector to obtain binary predictions. The function eval_metric() returns the accuracy metric (“accuracy”, “precision”, “recall” or “f1”) 
between a binary prediction time series and some known binary ground truth time series indicating the presence of anomalies.
