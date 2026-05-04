# Лабораторная работа №2

## Цель работы
Освоить процесс создания статического сайта с использованием генератора документации MkDocs.  
Научиться организовывать структуру документации проекта (портфолио лабораторных работ).  
Изучить базовые принципы работы с системой контроля версий Git и платформой GitHub.  
Развернуть статический сайт с использованием механизма GitHub Pages на домене вида username.github.io.  
Освоить базовую настройку темы оформления и конфигурационного файла mkdocs.yml.

## Задачи
- Создавать и преобразовывать массивы (вектор, матрица, reshape, транспонирование).
- Выполнять векторные операции (сложение, умножение на скаляр, поэлементное произведение, скалярное произведение).
- Реализовать матричные операции (умножение, определитель, обратная матрица, решение СЛАУ).
- Проводить статистический анализ данных (загрузка из CSV, расчёт статистик, нормализация).
- Строить и сохранять графики (гистограмма, тепловая карта, линейный график).

## Код программы

```python title="Lab_2.py" linenums="1"
import os
import numpy as np  # type: ignore
import matplotlib    # type: ignore
import pandas as pd  # type: ignore
import matplotlib.pyplot as plt  # type: ignore
import seaborn as sns  # type: ignore
matplotlib.use('Agg')

# TODO Создание и обработка массивов
def create_vector():
    return np.arange(10)

def create_matrix():
    return np.random.rand(5, 5)

def reshape_vector(vec):
    return vec.reshape(2, 5)

def transpose_matrix(mat):
    return np.transpose(mat)

# TODO Векторные операции
def vector_add(a, b):
    return a + b

def scalar_multiply(vec, scalar):
    return vec * scalar

def elementwise_multiply(a, b):
    return a * b

def dot_product(a, b):
    return np.dot(a, b)

# TODO Матричные операции
def matrix_multiply(a, b):
    return np.matmul(a, b)

def matrix_determinant(a):
    return np.linalg.det(a)

def matrix_inverse(a):
    return np.linalg.inv(a)

def solve_linear_system(a, b):
    return np.linalg.solve(a, b)

# TODO Статистический анализ
def load_dataset(path="data/students_score.csv"):
    return pd.read_csv(path).to_numpy()

def statistical_analysis(data):
    return {
        "mean": np.mean(data),
        "median": np.median(data),
        "std": np.std(data),
        "min": np.min(data),
        "max": np.max(data),
        "25_percentile": np.percentile(data, 25),
        "75_percentile": np.percentile(data, 75)
    }

def normalize_data(data):
    min_val = np.min(data)
    max_val = np.max(data)
    return (data - min_val) / (max_val - min_val)

# TODO Визуализация
def plot_histogram(data):
    plt.figure()
    plt.hist(data, bins=10, edgecolor='black')
    plt.title('Распределение оценок по математике')
    plt.xlabel('Оценка')
    plt.ylabel('Частота')
    os.makedirs('plots', exist_ok=True)
    plt.savefig('plots/histogram.png')
    plt.close()

def plot_heatmap(matrix):
    plt.figure(figsize=(6, 5))
    sns.heatmap(matrix, annot=True, cmap='coolwarm', center=0)
    plt.title('Корреляция предметов')
    os.makedirs('plots', exist_ok=True)
    plt.savefig('plots/heatmap.png')
    plt.close()

def plot_line(x, y):
    plt.figure()
    plt.plot(x, y, marker='o', linestyle='-')
    plt.title('Оценки студентов по математике')
    plt.xlabel('Номер студента')
    plt.ylabel('Оценка')
    os.makedirs('plots', exist_ok=True)
    plt.savefig('plots/line_plot.png')
    plt.close()
```

## Выводы
В ходе выполнения лабораторной работы №2 были освоены базовые возможности библиотек NumPy, Pandas и Matplotlib/Seaborn для работы с многомерными массивами, выполнения векторных и матричных операций, а также визуализации данных.