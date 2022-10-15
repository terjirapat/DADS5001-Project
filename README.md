# DADS5001_MiniProject_6510412009

Topic : วิเคราะห์ความคุ้มค่าในการซื้อคอนโดใน กทม.

Dataset : ข้อมูลอสังหาแต่ละโครงการในประเทศไทยโดยแสดง ที่ตั้ง ราคา พื้นที่และค่าเช่า เป็นต้น โดยเก็บรวบรวมข้อมูลจาก baania ซึ่งเป็นเว็ปไซต์ประกาศขายและเช่าอสังหาริมทรัพย์

Question : 
- ถ้าต้องการซื้อคอนโดเพื่อปล่อยเช่าที่ไหนที่ผลตอบแทนสูงที่สุด
- ราคาคอนโดต่อตารางเมตรของแต่ละเขตในกรุงเทพมีความสัมพันธ์กับความสะดวกในการเดินทางและอาหารการกินไหม
- ราคาที่ดินมีผลต่อราคาคอนโดต่อตารางเมตรไหม

Challenge : 
- ข้อมูลราคาค่าเช่าคอนโดที่ได้เป็นค่า median แยกตามเขต ซึ่งทำให้วิเคราะห์ผลตอบแทนจากค่าเช่ารายโครงการไม่ได้ต้องแยกตามเขตแทน
- ข้อมูลที่นำมาใช้เก็บรวบรวมจากเว็ป baania ซึ่งอาจไม่ครอบคลุมข้อมูลจริงทั้งหมด เช่น ข้อมูลราคาค่าเช่าคอนโดมีไม่ครบทุกเขตใน กทม. ซึ่งเกิดจากบางเขตไม่มีคนลงประกาศเช่าใน baania จึงไม่มีข้อมูล
- แต่ละโครงการมีห้องหลายราคาทำให้ยากต่อการเลือกข้อมูลมาใช้ เพราะ ถ้าเลือกใช้ทุกราคาจะทำให้ค่า mean เพี้ยนเนื่องจากจำนวนที่มากเกินไป เนื่องจากหัวข้อเป็นวิเคราะห์ความคุ้มค่าของการซื้อคอนโด จึงเลือกใช้ห้องราคาต่ำที่สุดของแต่ละโครงการและตัดข้อมูลห้องที่เหลือในโครงการทิ้งไป

# Importing Datasets
```
unit = pd.read_csv('opendata_unittype.csv')
proj = pd.read_csv('opendata_project.csv')
liv_sc = pd.read_csv('opendata_living_score.csv')
eat_sc = pd.read_csv('opendata_eating_score.csv')
rent_p = pd.read_csv('opendata_median_price_rent.csv')
```
- Filter เอาแต่ข้อมูลที่เป็นคอนโดใน กทม.
- แต่ละโครงการมีห้องหลายราคาทำให้ยากต่อการเลือกข้อมูลมาใช้ เพราะ ถ้าเลือกใช้ทุกราคาจะทำให้ค่า mean เพี้ยนเนื่องจากจำนวนที่มากเกินไป เนื่องจากหัวข้อเป็นวิเคราะห์ความคุ้มค่าของการซื้อคอนโด จึงเลือกใช้ห้องราคาต่ำที่สุดของแต่ละโครงการและตัดข้อมูลห้องที่เหลือในโครงการทิ้งไป
- Clean และ Merge เข้าด้วยกัน

# Final Dataset
```python
<class 'pandas.core.frame.DataFrame'>
Int64Index: 2368 entries, 0 to 2367
Data columns (total 21 columns):
 #   Column                    Non-Null Count  Dtype         
---  ------                    --------------  -----         
 0   project_id                2368 non-null   object        
 1   subdistrict_id            2368 non-null   float64       
 2   name_en                   2368 non-null   object        
 3   latitude                  2368 non-null   float64       
 4   longitude                 2368 non-null   float64       
 5   subdistrict_name_en       2368 non-null   object        
 6   subdistrict_name_th       2368 non-null   object        
 7   district_name_en          2368 non-null   object        
 8   district_name_th          2368 non-null   object        
 9   province_name_en          2368 non-null   object        
 10  province_name_th          2368 non-null   object        
 11  area_usable_min           2368 non-null   float64       
 12  price_min                 2368 non-null   float64       
 13  date_updated              2368 non-null   datetime64[ns]
 14  pricesqm                  2368 non-null   float64       
 15  eating_price_score        2368 non-null   float64       
 16  eating_quality_score      2368 non-null   float64       
 17  living_score              2368 non-null   float64       
 18  listing_district_name_en  2204 non-null   object        
 19  total_listing             2204 non-null   float64       
 20  median_rent_price_sqm     2204 non-null   float64       
dtypes: datetime64[ns](1), float64(11), object(9)
memory usage: 407.0+ KB
```

# EDA
### Condo Location
![image](https://user-images.githubusercontent.com/77285026/195904699-b9af3569-3ada-46cd-91b1-251a7bf560ef.png)

### Number of Condos by Districts
![image](https://user-images.githubusercontent.com/77285026/195904997-f15c9a06-a826-46d6-94b9-d58e1c9b6df4.png)
![image](https://user-images.githubusercontent.com/77285026/195905075-8d7762c8-c1f6-45a1-88fe-abd7045d1d7e.png)

### Condo Price Per SQM
![image](https://user-images.githubusercontent.com/77285026/195905235-5cd02648-c1b4-4e3e-b97e-1c306f5ab35c.png)
![image](https://user-images.githubusercontent.com/77285026/195905718-4a1105f1-cad0-43c7-a586-bfafc92d3373.png)
![image](https://user-images.githubusercontent.com/77285026/195905614-bdfbf3ea-b684-4c01-a6c4-540f0b7a6303.png)
![image](https://user-images.githubusercontent.com/77285026/195882672-0d4b4fe7-d39c-4384-b04e-8d99f2e6acd8.png)

# Q&A
## Q1: ถ้าต้องการซื้อคอนโดเพื่อปล่อยเช่าที่ไหนที่ผลตอบแทนสูงที่สุด?
ข้อมูลที่ใช้
- Condo Rent Price เลือกใช้ค่าเช่าที่มีข้อมูลล่าสุดคือ เดือน 1 ปี 2022
- Condo Price ข้อมูลราคาคอนโด Updated ล่าสุดของแต่ละโครงการ
- Condo Price Per SQM

*ข้อมูลค่าเช่าเป็น Median และข้อมูลค่าเช่าไม่มีแบบรายโครงการจึงต้องใช้รายเขตแทน

### Rent Return by Districts
![image](https://user-images.githubusercontent.com/77285026/195891800-39cfbdb0-d8dd-4ac6-82da-f2636868021f.png)
![image](https://user-images.githubusercontent.com/77285026/195891704-5a5bd723-8316-440a-858e-bc286f89ebff.png)

![image](https://user-images.githubusercontent.com/77285026/195891385-80d5e168-308d-4c96-9421-5923398aec37.png)

### Price Per SQM by Districts
![image](https://user-images.githubusercontent.com/77285026/195905614-bdfbf3ea-b684-4c01-a6c4-540f0b7a6303.png)

เขตที่มีผลตอบแทนสูงที่สุด ไม่ใช่เขตที่ราคาคอนโดต่อตารางเมตรแพงที่สุด

### A1: บางกะปิ ลาดพร้าว ลาดกระบัง มีผลตอบแทนสูงที่สุด 9.21% 9.20% 8.70% ตามลำดับ

## Q2: ราคาคอนโดต่อตารางเมตรของแต่ละเขตในกรุงเทพมีความสัมพันธ์กับความสะดวกในการเดินทางและอาหารการกินไหม
ข้อมูลที่ใช้
- Living Score คะแนนด้านความสะดวกในการเดินทาง ยิ่งคะแนนสูงความสะดวกในการเดินทางยิ่งมาก
- Eating Quality Score คะแนนด้านคุณภาพอาหาร ยิ่งคะแนนสูงคุณภาพอาหารยิ่งมาก
- Eating Price Score คะแนนด้านราคาอาหาร ยิ่งคะแนนสูงราคาอาหารยิ่งแพง
- Condo Price Per SQM

### Map
![image](https://user-images.githubusercontent.com/77285026/195907913-fd18dda8-be83-44ea-820d-58993f045819.png)
![image](https://user-images.githubusercontent.com/77285026/195907936-0b505538-de08-4e06-aa43-261cb004b772.png)
![image](https://user-images.githubusercontent.com/77285026/195907960-82956d33-d70a-44f9-be24-e594ee4b2586.png)

### Scatter
![image](https://user-images.githubusercontent.com/77285026/195904076-5eda2f51-3856-4183-a9d2-9da96d0069a4.png)
เทียบ Living Score กับ Condo Price Per SQM โดยสีกำหนดโดยสัดส่วน Living Score ที่ได้ต่อ Condo Price Per SQM
พบว่า Living Score ไปในทางเดียวกับ Condo Price Per SQM แต่เมื่อเทียบสัดส่วนพบว่าราคาต่ำจะมีสัดส่วนที่สูงกว่าราคาสูง สรุปได้ว่าทั้งสองค่าไปในทิศทางเดียวกันแต่เมื่อราคาสูงถึงจุดหนึ่งอาจได้ประโยชน์จากส่วนอื่นที่เพิ่มขึ้นมาแทน

![image](https://user-images.githubusercontent.com/77285026/195904117-574519b8-7fdb-4fca-8aed-47b1f23e2884.png)

### A2: เขตที่ราคาคอนโดต่อตารางเมตรสูงยิ่งสะดวกในการเดินทางและคุณภาพอาหารที่ดีแต่ราคาอาหารจะแพง เทียบกับเขตที่ราคาคอนโดต่อตารางเมตรต่ำความสะดวกในการเดินทางและคุณภาพอาหารจะน้อยลงแต่ราคาอาหารจะถูกกว่า

## Q3: ราคาที่ดินมีผลต่อราคาคอนโดต่อตารางเมตรไหม
ข้อมูลที่ใช้
- Condo Price Per SQM
- Bangkok Land Values

![image](https://user-images.githubusercontent.com/77285026/195898795-ab386320-9f4b-4717-8433-2ccb4a41e942.png)
![image](https://user-images.githubusercontent.com/77285026/195898878-ad1b6a38-bb6e-4cb6-aad5-39b95ba75357.png)
### A3: มีผล เพราะ พื้นที่ที่ราคาที่ดินสูงมีราคาคอนโดต่อตารางเมตรที่สูงตาม

# Summary
- ถ้าซื้อคอนโดเพื่อปล่อยเช่าควรเลือกซื้อเขตบางกะปิ ลาดพร้าวและลาดกระบัง
- ถ้าซื้อคอนโดเพื่ออยู่เอง พิจารณาจากคอนโดที่อยู่ในพื้นที่ราคาแพงจะมีความสะดวกในการเดินทาง คุณภาพร้านอาหารที่ดี แต่อาหารราคาแพง

# Data Source
## ข้อมูลอสังหา
### Bestimate
- https://gobestimate.com/data-detail/Residental-Project-Data
- https://gobestimate.com/data-detail/Residental-Unittype-Data
- https://gobestimate.com/data-detail/Living-Score-By-Location
- https://gobestimate.com/data-detail/Eating-Score-By-Location
- https://gobestimate.com/data-detail/Median-Rental-Price-by-Location
## ข้อมูลแผนที่
### Bangkok GIS
- http://www.bangkokgis.com/modules.php?m=download_shapefile
