# DeepECG Docker

Анализ ЭКГ сигналов с использованием deep learning моделей. Поддерживает классификацию по 77 диагнозам, а также бинарную классификацию риска мерцательной аритмии (AFIB) и сниженной фракции выброса левого желудочка (LVEF).

## Требования

- Docker Desktop
- Git
- HuggingFace API key

---

## Установка и запуск

### 1. Клонирование репозитория

```bash
git clone https://github.com/JKurdina/DeepLerning_ECGDocker.git
cd DeepLerning_ECGDocker
```

### 2. Настройка HuggingFace API ключа

Зарегистрироваться на HuggingFace

Вставить ключ в файл `api_key.json`:

```json
{
  "HUGGING_FACE_API_KEY": "hf_xxxxxxxxxxxxxxxxx"
}
```

### 3. Сборка Docker образа

```bash
docker build -t deepecg-docker .
```

### 4. Запуск контейнера

**PowerShell:**
```powershell
docker run -d --name deepecg `
  -v ${PWD}/inputs:/app/inputs `
  -v ${PWD}/outputs:/app/outputs `
  -v ${PWD}/ecg_signals:/app/ecg_signals:ro `
  -v ${PWD}/preprocessing:/app/preprocessing `
  -v ${PWD}/thresholds:/app/thresholds `
  -v ${PWD}/weights:/app/weights `
  -v ${PWD}/results:/app/results `
  -v ${PWD}/tests:/app/tests `
  deepecg-docker
```

**Windows CMD:**
```cmd
docker run -d --name deepecg -v %cd%/inputs:/app/inputs -v %cd%/outputs:/app/outputs -v %cd%/ecg_signals:/app/ecg_signals:ro -v %cd%/preprocessing:/app/preprocessing -v %cd%/thresholds:/app/thresholds -v %cd%/weights:/app/weights -v %cd%/results:/app/results -v %cd%/tests:/app/tests deepecg-docker
```

**Git Bash / macOS / Linux:**
```bash
docker run -d --name deepecg \
  -v $(pwd)/inputs:/app/inputs \
  -v $(pwd)/outputs:/app/outputs \
  -v $(pwd)/ecg_signals:/app/ecg_signals:ro \
  -v $(pwd)/preprocessing:/app/preprocessing \
  -v $(pwd)/thresholds:/app/thresholds \
  -v $(pwd)/weights:/app/weights \
  -v $(pwd)/results:/app/results \
  -v $(pwd)/tests:/app/tests \
  deepecg-docker
```

### 5. Подключение к контейнеру

```bash
docker exec -it deepecg bash
```

### 6. Запуск пайплайна

```bash
bash run_pipeline.bash --mode full_run --csv_file_name data_rows_template_npy.csv
```

---

## 7. Проверка результатов

В репозитории есть эталонные результаты в папке `results/` и скрипт сравнения `tests/compare_results.py`.

### Запуск проверки

```bash
python tests/compare_results.py
```

### Ожидаемый вывод

```
============================================================
DeepECG — сравнение результатов с эталоном
============================================================

[1/3] Проверка препроцессинга...
  OK  ecg_processing_detailed_report

[2/3] Проверка вероятностей wcr_77_classes...
  OK  wcr_77_classes_ecg_machine_diagnosis_probabilities

[3/3] Проверка вероятностей wcr_lvef_equal_under_40...
  OK  wcr_lvef_equal_under_40_lvef_40_probabilities

============================================================
```

> Допустимое отклонение вероятностей между запусками: ±0.1

---
## Остановка контейнера

```bash
docker stop deepecg
docker rm deepecg
```

---
