# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнила:
- Нестерова Анастасия Андреевна
- РИ210947
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Ознакомиться с основными операторами зыка Python на примере реализации линейной регрессии.

## Задание 1
### В проекте Unity реализовать перцептрон, который умеет производить вычисления.
Ход работы:
1) OR | дать комментарии о корректности работы

Подставляем такие значения, чтобы выполнялась логическая операция OR. Данные перцептрона совпадают с заведёнными нами.
![OR](https://user-images.githubusercontent.com/43472988/204355685-f59d4976-4d92-4fbf-9a7a-6e7bbe8ad2af.jpg)

2) AND | дать комментарии о корректности работы

Делаем всё тоже самое, подставляем такие значения, чтобы выполнялась логическая операция AND. Данные перцептрона совпадают с заведёнными нами.
![AND](https://user-images.githubusercontent.com/43472988/204355880-6ebd0b15-707a-425e-993d-3130dd8e3055.jpg)

3) NAND | дать комментарии о корректности работы

Подставляем такие значения, чтобы выполнялась логическая операция NAND. Данные перцептрона совпадают с заведёнными нами.
![NAND](https://user-images.githubusercontent.com/43472988/204356509-fd71e3fb-3fa3-4c57-8689-e43bd4a628dc.jpg)

4) XOR | дать комментарии о корректности работы

И наконец gодставляем такие значения, чтобы выполнялась логическая операция XOR. Как видно в приведённом ниже скрине, данные перцептрона не совпадают с данными, введёнными нами.
![XOR](https://user-images.githubusercontent.com/43472988/204356647-59b081a8-7795-4920-a5e4-1c15a9071c7a.jpg)

Попытаемся в скрипте изменить колличество эпох с 8 на 15. Наблюдаем, что ситуация не изменилась, и данные, которые должны получиться после выполнения логической операции OR, получаются одними, а данные перцептрона - другими. Следовательно данная операция нелинейноотделимая, и перыептрон не может найти правильные данные.
![Перемена](https://user-images.githubusercontent.com/43472988/204360105-0b6cada3-d7da-4c2a-b176-34daa056f108.jpg)

## Задание 2

## Построить графики зависимости количества эпох от ошибки обучения. Указать от чего зависит необходимое количество эпох обучения.

Для создания графиков я собрала данные благодаря проеку в Unity. Меняя количество эпох, я получала различные значения для каждой логической операции , которые и записывала в таблицу, после чего создала диаграмму по этой таблице.

![Снимок экрана 2022-11-29 в 02 04 19](https://user-images.githubusercontent.com/43472988/204380921-ee47f941-b923-4d37-9d7d-4b8df65c63f3.png)

Рассмотрев таюлицу, её данные и графики, а также изучив лекцию, я сделала вывод, что необходимое количество эпох обучения зависит от значений bias(смещение) и weights(вес). Нижеприведённая часть данного код работы перцептрона является подтверждением данной гипотезы, а в частности методы DotProductBias и CalcOutput:

```
double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}
```
  
## Задание 3

## Построить визуальную модель работы перцептрона на сцене Unity.

Визуализация заключается в смене цвета объекта при его соприкосновении с поверхностью. Цвет зависит от результата логической операции: если получается 0, то цвет меняется на красный, а если же 1 - на зелёный.
Преобразованный код:

```
[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour {

	public int num1, num2; //создаём две переменные для чисел, к которым применяются логические операции

	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f);
		}
		bias = Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; 
		}
		bias += error;
	}

	public double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train(int epochs)
	{
		InitialiseWeights();
		
		for(int e = 0; e < epochs; e++)
		{
			totalError = 0;
			for(int t = 0; t < ts.Length; t++)
			{
				UpdateWeights(t);
				Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
			}
			Debug.Log("TOTAL ERROR: " + totalError);
		}
	}

	void Start () {
		Train(8);
		var value = CalcOutput(num1, num2);
		ChangeColor.OnSearchValue(value);
		Debug.Log("Test 0 0: " + CalcOutput(num1, num2));	
	}
//также создаём класс перемены цвета	
public class ChangeColor : MonoBehaviour
{
    static double value = 0;

    public void OnTriggerEnter(Collider other) {
    	//если итоговое значение равно 0, то объект меняет цвет на красный, а если 1 - на зелёный.
        if (value == 0)
            other.gameObject.GetComponent<Renderer>().material.color = Color.red;
        else
            other.gameObject.GetComponent<Renderer>().material.color = Color.green;
    }

    public static void OnSearchValue(double answer) {
        value = answer;
    }
}
```

На скринах ниже можно увидеть результат работы данного кода на примере операции NAND.
1) num1 = 0, num2 = 0:

![00](https://user-images.githubusercontent.com/43472988/204391750-c822856e-a796-40a3-9300-4086d13531de.jpg)

2) num1 = 0, num2 = 1:

![01](https://user-images.githubusercontent.com/43472988/204391785-954a7e11-5f1d-4316-a976-3bd7669ffc70.jpg)

3) num1 = 1, num2 = 0:

![10](https://user-images.githubusercontent.com/43472988/204391830-e5292b60-198c-458c-b1d8-931fb48ac354.jpg)

5) num1 = 1, num2 = 1:

![11](https://user-images.githubusercontent.com/43472988/204391886-c12cb87e-c40b-458e-ba09-5a7505a51e82.jpg)
	
## Выводы
Лабораторная работа познакомила меня с такой моделью, как перцептрон. Я смогла понаблюдать за его принципами и работой, изучила код на python. Также я поработала с различными логическими операциями и визуализировала их работу в проекте Unity. 

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
