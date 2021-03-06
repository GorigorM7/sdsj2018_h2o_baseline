# Sberbank Data Science Journey 2018: H2O AutoML Baseline

Бейзлайн к соревнованию [SDSJ 2018 AutoML](http://sdsj.sberbank.ai/) c использованием H2O AutoML.

Документация и примеры по H2O AutoML:
- http://docs.h2o.ai/h2o/latest-stable/h2o-docs/automl.html
- http://docs.h2o.ai/h2o/latest-stable/h2o-py/docs/modeling.html#h2oautoml
- https://github.com/h2oai/h2o-tutorials/blob/master/h2o-world-2017/automl/Python/automl_regression_powerplant_output.ipynb

H2O AutoML в рамках отведенного лимита времени строит на данных ряд моделей из списка
- GLM - generalized linear models
- GBM - градиентный бустинг
- DRF(Distributed Random Forest) - Random Forest и Extremely Randomized Trees
- Deep learning

Для предсказания выбирается лучшая модель.
Имеются два режима обучения - с кросс-валидацией и без. С кросс-валидацией на основе базовых моделей строится ансамбль - Stacked Ensemble, который, как правило, и будет лучшей моделью.

Бонус - в файле validate.py тестирование на локальных датасетах на основе бейзлайна https://github.com/vlarine/sdsj2018_lightgbm_baseline, но с добавлением логирования экспериментов с помощью [mlflow](https://mlflow.org/).
mlflow позволяет сохранять параметры, результаты и исходный код экспериментов, смотреть и сравнивать результаты в веб-интерфейсе. Подробнее
https://mlflow.org/docs/latest/tutorial.html


---

#### Возможные пути улучшения

- Выбирать режим в зависимости от размера датасета - для небольших датасетов выполнять  кросс-валидацию со стэкингом, для больших датасетов обучаться без кросс-валидации.
- Есть параметр exclude_algos. Он позволяет настраивать список алгоритмов, которые используются в обучении. Можно, например, ограничить используемые алгоритмы только бустингом, чтобы не тратить время на другие алгоритмы (как правило, бустинг все равно оказывается лучшим вариантом). Либо наоборот, для совсем маленьких датасетов попробовать использовать менее склонные к переобучению алгоритмы - GLM, DRF.
- По умолчанию H2O делит train на train/validation/test(leaderboard) в пропорции 80%/10%/10%. Возможно, 10% на тест в каких-то задачах - слишком мало и приводит к подгонке к нему, можно руками задавать validation_frame и leaderboard_frame (тест, на котором скорятся модели и выбирается лучшая). 


---

### Update

Текущая версия работает с восьмым (самым большим) датасетом.
