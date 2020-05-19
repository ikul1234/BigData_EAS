## Bigdata 2020

# Tugas 7 Big DATA

- [Tugas 7 Big DATA](#tugas-7-big-data)
- [Daily Minimum Temperature](#daily-minimum-temperature)
  - [Business Understanding](#business-understanding)
  - [Data Understanding](#data-understanding)
  - [Data Preparation](#data-preparation)
  - [Modeling](#modeling)
  - [Deployment](#deployment)
- [Electricity Production](#electricity-production)
  - [Business Understanding](#business-understanding-1)
  - [Data Understanding](#data-understanding-1)
  - [Data Preparation](#data-preparation-1)
  - [Modeling](#modeling-1)
  - [Deployment](#deployment-1)

# Daily Minimum Temperature

## Business Understanding
Dilihat dari judul dataset, data ini berupa data temperatur terendah di dalam jangka waktu harian

## Data Understanding
Data mengandung 3500~ row dan  memiliki 2 kolom, yaitu :
- Date
- Daily minimum temperature


## Data Preparation
- Pertama, dari tugas 7 ganti table file reader dan ganti nama kolom seperti gambar di bawah</br>
  ![alt text](https://github.com/ikul1234/BigData_EAS/blob/master/Screenshot/1.jpg "DPPertama")<br/>
- Kedua, tambah node RowID di dalam komponen Load Data</br>
  ![alt text](https://github.com/ikul1234/BigData_EAS/blob/master/Screenshot/2.jpg  "DPKedua")<br/>
- Ketiga, atur node RowID seperti gambar di bawah</br>
![alt text](https://github.com/ikul1234/BigData_EAS/blob/master/Screenshot/3.jpg "DPKetiga")<br/>
- Keempat, ubah query pada Spark SQL Query pertama seperti di bawah ini</br>
```
SELECT 

meterID,
enc_datetime,
reading as kw30,
date_add(to_date(cast(unix_timestamp(enc_datetime,"MM/dd/yyyy") as timestamp)),1) as eventDate

FROM #table# t1
```
- Kelima, ubah query pada Spark SQL Query kedua seperti di bawah ini</br>
```
SELECT 

meterID,
kw30,
eventDate,
year(eventDate) as year,
month(eventDate) as month,
weekofyear(eventDate) as week,
date_format(eventDate, 'EEEE') as dayOfWeek

FROM #table# t1
```
- Keenam, ubah query pada Spark SQL Query ketiga seperti di bawah ini dan hapus Spark SQL Query keempat</br>
```
SELECT *, 
CASE 
WHEN dayOfWeek in ('Saturday','Sunday') 	THEN 'WE' 
									        ELSE 'BD' 
END as dayClassifier

from #table#
```
- Ketujuh, hapus node di dalam component Aggregation yang ditandai pada gambar di bawah dan sambungkan kembali joiner yang putus</br>
![alt text](https://github.com/ikul1234/BigData_EAS/blob/master/Screenshot/4.jpg"DPKetujuh")<br/>
- Kedelapan, ubah query pada Spark SQL Query di sebelah componen Aggregation seperti di bawah</br>
```
SELECT `meterID`, `totalKW`, `avgYearlyKW`,`avgMonthlyKW`,`avgWeeklyKW`,
       `avgMonday`,`avgTuesday`,`avgWednesday`,`avgThursday`,`avgFriday`,`avgSaturday`,`avgSunday`,
       `avgDaily`,`avg_BD`,`avg_WE`,
       (avgMonday / avgWeeklyKW) * 100.0 as pctMonday,
       (avgTuesday / avgWeeklyKW) * 100.0 as pctTuesday,
       (avgWednesday / avgWeeklyKW) * 100.0 as pctWednesday,
       (avgThursday / avgWeeklyKW) * 100.0 as pctThursday,
       (avgFriday / avgWeeklyKW) * 100.0 as pctFriday,
       (avgSaturday / avgWeeklyKW) * 100.0 as pctSaturday,
       (avgSunday / avgWeeklyKW) * 100.0 as pctSunday
       
FROM #table#
```
- Kesembilan, tambahkan node Spark Missing Value dan atur nodenya seperti gambar di bawah</br>
![alt text](https://github.com/ikul1234/BigData_EAS/blob/master/Screenshot/5.jpg"DPKesembilan")<br/>
![alt text](https://github.com/ikul1234/BigData_EAS/blob/master/Screenshot/6.jpg"DPKesembilan")<br/>

## Modeling
- Pertama, pada component PCA atur node Spark Normalizer dan Spark PCA seperti gambar di bawah</br>
![alt text](https://github.com/ikul1234/BigData_Tugas5/blob/master/Screenshot/7.jpg"MPertama")<br/>
- Kedua, jalankan seluruhnya</br>
![alt text](https://github.com/ikul1234/BigData_Tugas5/blob/master/Screenshot/8.jpg"MKedua")<br/>


## Deployment
- Berikut adalah hasil clustering data</br>
![alt text](https://github.com/ikul1234/BigData_Tugas5/blob/master/Screenshot/9.jpg"DPertama")<br/>


# Electricity Production

## Business Understanding
Dilihat dari judul dataset, data ini berupa data produksi listrik per harinya

## Data Understanding
Data mengandung 300~ row dan  memiliki 2 kolom, yaitu :
- Date
- IPG2211A2N


## Data Preparation
- Pertama, dari tugas 7 ganti table file reader dan ganti nama kolom </br>
  ![alt text](https://github.com/ikul1234/BigData_EAS/blob/master/Screenshot/1.jpg "DPPertama")<br/>
- Kedua, tambah node RowID di dalam komponen Load Data</br>
  ![alt text](https://github.com/ikul1234/BigData_EAS/blob/master/Screenshot/2.jpg  "DPKedua")<br/>
- Ketiga, atur node RowID seperti gambar di bawah</br>
![alt text](https://github.com/ikul1234/BigData_EAS/blob/master/Screenshot/3.jpg "DPKetiga")<br/>
- Keempat, ubah query pada Spark SQL Query pertama seperti di bawah ini</br>
```
SELECT 

meterID,
enc_datetime,
reading as kw30,
date_add(to_date(cast(unix_timestamp(enc_datetime,"MM/dd/yyyy") as timestamp)),1) as eventDate

FROM #table# t1
```
- Kelima, ubah query pada Spark SQL Query kedua seperti di bawah ini</br>
```
SELECT 

meterID,
kw30,
eventDate,
year(eventDate) as year,
month(eventDate) as month,
weekofyear(eventDate) as week,
date_format(eventDate, 'EEEE') as dayOfWeek

FROM #table# t1
```
- Keenam, ubah query pada Spark SQL Query ketiga seperti di bawah ini dan hapus Spark SQL Query keempat</br>
```
SELECT *, 
CASE 
WHEN dayOfWeek in ('Saturday','Sunday') 	THEN 'WE' 
									        ELSE 'BD' 
END as dayClassifier

from #table#
```
- Ketujuh, hapus node di dalam component Aggregation yang ditandai pada gambar di bawah dan sambungkan kembali joiner yang putus</br>
![alt text](https://github.com/ikul1234/BigData_EAS/blob/master/Screenshot/4.jpg"DPKetujuh")<br/>
- Kedelapan, ubah query pada Spark SQL Query di sebelah componen Aggregation seperti di bawah</br>
```
SELECT `meterID`, `totalKW`, `avgYearlyKW`,`avgMonthlyKW`,`avgWeeklyKW`,
       `avgMonday`,`avgTuesday`,`avgWednesday`,`avgThursday`,`avgFriday`,`avgSaturday`,`avgSunday`,
       `avgDaily`,`avg_BD`,`avg_WE`,
       (avgMonday / avgWeeklyKW) * 100.0 as pctMonday,
       (avgTuesday / avgWeeklyKW) * 100.0 as pctTuesday,
       (avgWednesday / avgWeeklyKW) * 100.0 as pctWednesday,
       (avgThursday / avgWeeklyKW) * 100.0 as pctThursday,
       (avgFriday / avgWeeklyKW) * 100.0 as pctFriday,
       (avgSaturday / avgWeeklyKW) * 100.0 as pctSaturday,
       (avgSunday / avgWeeklyKW) * 100.0 as pctSunday
       
FROM #table#
```
- Kesembilan, tambahkan node Spark Missing Value dan atur nodenya seperti gambar di bawah</br>
![alt text](https://github.com/ikul1234/BigData_EAS/blob/master/Screenshot/5.jpg"DPKesembilan")<br/>
![alt text](https://github.com/ikul1234/BigData_EAS/blob/master/Screenshot/6.jpg"DPKesembilan")<br/>

## Modeling
- Pertama, pada component PCA atur node Spark Normalizer dan Spark PCA seperti gambar di bawah</br>
![alt text](https://github.com/ikul1234/BigData_Tugas5/blob/master/Screenshot/7.jpg"MPertama")<br/>
- Kedua, jalankan seluruhnya</br>
![alt text](https://github.com/ikul1234/BigData_Tugas5/blob/master/Screenshot/8.jpg"MKedua")<br/>


## Deployment
- Berikut adalah hasil clustering data</br>
![alt text](https://github.com/ikul1234/BigData_Tugas5/blob/master/Screenshot/10.jpg"DPertama")<br/>