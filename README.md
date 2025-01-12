# Deep Learning-based Drum Sound Classification with ZCR and Spectral Centroid

โปรเจคนี้มุ้งเน้นการใช้ Deep Learning จำแนกเสียงกลองประเภท Kick Drum และ Snare Drum เป็นการประยุกต์ใช้เทคนิคสมัยใหม่ในการเรียนรู้จากข้อมูลเสียงที่สกัดfeature เช่น Zero Crossing Rate (ZCR) และ Spectral Centroid ที่ช่วยเพิ่มความแม่นยำในการจำแนกเสียงโดย มีขั้นตอนดังนี้
<p align="center">
  <img width="200" height="200" src="https://github.com/NoppalitP/Classification_drum_sounds/blob/main/kck.png">
</p>
<p align="center">
  <img width="200" height="200" src="https://github.com/NoppalitP/Classification_drum_sounds/blob/main/snare.png">
</p>

## 1.  การเตรียมข้อมูล (Data Preparation):

* **การบันทึกเสียง**: เสียงกลองประเภท **Kick Drum** และ **Snare Drum** จะถูกบันทึกจากแหล่งต่างๆ หรือจากคลังข้อมูลเสียงที่มีอยู่
* **feature extraction**: ใช้ Library **librosa** ในการทำสกัด feature **Zero Crossing Rate (ZCR)** และ **Spectral Centroid** เพื่อสร้างฟีเจอร์จากเสียง
*   1. Zero Crossing Rate (ZCR)** คือจำนวนครั้งที่สัญญาณเสียงข้ามแกนศูนย์ **(zero-cross)** ในช่วงเวลาหนึ่ง ค่าของ **ZCR** เป็นตัวชี้วัดความถี่ที่สัญญาณมีการเปลี่ยนแปลงทิศทาง (จากบวกเป็นลบ หรือจากลบเป็นบวก) ซึ่งจะช่วยบ่งบอกถึงความ "คมชัด" ของเสียง เช่น เสียงที่มี **ZCR** สูงอาจบ่งบอกถึงเสียงกลองที่มีการเปลี่ยนแปลงมากในเวลาสั้นๆ และ

<p align="center">
  <img src="https://github.com/NoppalitP/Classification_drum_sounds/blob/main/hist_ZCR.png">
</p>
       
*   2. Spectral Centroid** เป็นการวัดตำแหน่งเฉลี่ยของพลังงานในสเปกตรัมความถี่ของเสียง โดยจะช่วยบ่งบอกถึง "ความสูง" ของเสียง หากค่าของ **Spectral Centroid** สูงแสดงว่าเสียงจะมีความถี่สูง (เสียงแหลม) หากต่ำจะหมายถึงเสียงมีความถี่ต่ำ (เสียงทุ้ม) ซึ่งเป็นคุณลักษณะที่สำคัญในการจำแนกเสียงกลองต่างๆ

<p align="center">
  <img src="https://github.com/NoppalitP/Classification_drum_sounds/blob/main/hist_sc.png">
</p>

<p align="center">
  <img src="https://github.com/NoppalitP/Classification_drum_sounds/blob/main/zcr_vs_cr.png">
</p>

## 2. การสร้างและฝึกฝนโมเดล Deep Learning:
ในขั้นตอนนี้ จะสร้างโมเดล Deep Learning ที่มีการสกัดคุณลักษณะ 2 ตัว (เช่น Zero Crossing Rate และ Spectral Centroid) แล้วใช้ Dense Neural Network พร้อมฟังก์ชันกระตุ้น LeakyReLU และการใช้ Dropout เพื่อลดการ overfitting จากนั้นเราจะฝึกโมเดลด้วยข้อมูลที่มีการจำแนกประเภท Kick Drum และ Snare Drum เพื่อให้โมเดลสามารถทำนายประเภทของเสียงกลองได้
<p align="center">
  <img src="https://github.com/NoppalitP/Classification_drum_sounds/blob/main/model.png">
</p>


## 3. การฝึกโมเดล:
* ใช้ข้อมูลที่มีการสกัดคุณลักษณะเพื่อฝึกโมเดล โดยข้อมูลจะถูกแบ่งเป็นชุดฝึก (Training set) และชุดทดสอบ (Test set)
* Loss Function: ใช้ Categorical Crossentropy ซึ่งเหมาะสมสำหรับปัญหาการจำแนกประเภท (classification) แบบหลายประเภท
* Optimizer: เช่น Adam Optimizer เพื่อให้โมเดลเรียนรู้และปรับปรุงน้ำหนักของแต่ละชั้นให้ดีขึ้น


## 4. การทดสอบและการประเมินผล:
* หลังจากฝึกโมเดลเสร็จแล้ว จะทดสอบความแม่นยำของโมเดลบนข้อมูลที่ไม่เคยเห็นมาก่อน (test data)
* ใช้ Accuracy, Precision, Recall, และ F1-score เพื่อประเมินผลการทำงานของโมเดล

## 5. ผลลัพธ์:
* โมเดลจะสามารถจำแนกเสียงกลองประเภท Kick Drum และ Snare Drum ได้อย่างแม่นยำจากข้อมูลเสียงที่ได้รับ โดยการใช้คุณลักษณะที่สกัดมาจาก ZCR, Spectral Centroid และ Spectrogram
* โมเดลนี้สามารถนำไปประยุกต์ใช้ในระบบการจำแนกเสียงในแอปพลิเคชันต่างๆ เช่น การสร้างแอปพลิเคชันที่ช่วยแยกแยะเสียงกลองในการบันทึกดนตรี หรือในการจำแนกเสียงในงานวิจัยด้านดนตรี

<p align="center">
  <img src="https://github.com/NoppalitP/Classification_drum_sounds/blob/main/loss.png">
</p>
<p align="center">
  <img src="https://github.com/NoppalitP/Classification_drum_sounds/blob/main/acc.png">
</p>
<p align="center">
  <img src="https://github.com/NoppalitP/Classification_drum_sounds/blob/main/report.png">
</p>
