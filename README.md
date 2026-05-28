# MelodyMelonTap Clean Stable v19

Clean rebuild: ตัดโค้ดซ้ำ/โค้ดเก่า/เอฟเฟกต์ออก เหลือระบบหลักที่เสถียร


## v19.1 Audio Retry Fix
- ปรับเงื่อนไข “ผลการวัดยังไม่นิ่ง” ไม่ให้เข้มเกินไป
- หากสัญญาณไม่นิ่งเล็กน้อย แอปจะแสดงผลต่อ พร้อมหมายเหตุให้วัดซ้ำเพื่อยืนยัน
- จะบังคับวัดใหม่เฉพาะกรณีสัญญาณแย่มากจริง ๆ


## v20 Raw2 Logic Restore
- กลับมาใช้สมการ Brix/Firmness/Juice/Hollow และ classifyRipenessAI จากชุด Raw2/v7 ซึ่งผู้ทดสอบพบว่าแม่นกว่า
- คง UI clean, 5 taps, audiofix และ AI Voice แบบธรรมชาติไว้
- ยกเลิกการใช้ Perfect Ripe เป็นตัวตัดสินระดับการสุกหลัก

## Voice Fix OK
- ปรับ AI Voice ให้ไม่อ่านคำว่า “ความกรอบ”
- ใช้คำว่า “เนื้อแน่น...” จาก crispText โดยตรง
- เว้นจังหวะด้วย comma
- ปรับความเร็วเสียงเป็น rate = 0.96
- Version: v2026.05.12-raw2-logic-v20-voicefix


## v22 Hybrid Audio120
- ใช้โครงสร้าง UI/Voice แบบ v20 ที่เสถียร
- ใส่สมการ RidgeCV จากข้อมูล Audio120
- เพิ่ม feature เสียง deployable: band energy, rolloff, peak count, peak2 ratio, attack/decay
- ใช้ Logistic Regression จาก Audio120 สำหรับระดับสุก

## v22.1 Ripeness Fix
- แก้ระดับการสุกไม่ตรง โดยไม่ใช้ Audio120 classifier เป็นตัวตัดสินหลัก
- ใช้ Raw2/v20 classifyRipenessAI เป็นตัวหลัก เพราะเสถียรกว่าในสนามจริง
- Audio120 ยังใช้ทำนาย Brix/Firmness/Juice/Hollow และแสดงผลใน debug
- เพิ่ม guard rule กันกรณีแอปบอก “ดิบ” ทั้งที่ Brix/Juice/Hollow ชี้ว่าใกล้สุกหรือสุกพอดี

## v22.3 Export Near Debug
- ย้ายปุ่ม Export CSV ไปไว้ใกล้ส่วนข้อมูลเชิงเทคนิค / Debug
- เพิ่มปุ่มล้างข้อมูล CSV ที่บันทึกในเครื่อง
- ใช้สำหรับ field test 50 ลูก

## v22.4 Share Export All
- ปุ่มแชร์ผลลัพธ์เปลี่ยนเป็น แชร์/Export ผล
- กดปุ่มเดียวแล้วแชร์ข้อความสรุป พร้อมดาวน์โหลด CSV ข้อมูลดิบทั้งหมด
- ยังมีปุ่มล้างข้อมูล CSV ใกล้ Debug

## v22.5 Field Calibration
- Calibrated with field-test 70 samples
- Brix +3.447
- Juice +10.396
- Hollow -3.856
- Firmness conservative correction -5.5 N
- Export CSV keeps raw and calibrated values

## v22.6 Ripeness + Hollow Field Fix
- ปรับระดับการสุกจาก field test + ค่าจริงภาพหน้าตัด
- ไม่ใช้ Audio120 classifier เป็นตัวตัดสินหลัก เพราะ field use ยังไวเกินไป
- ใช้ rule จาก feature ที่เห็นชัด: band_150_300, Q, Brix, Firmness, Juice
- ปรับ Hollow โดยคำนึงถึงหน่วยจริงใน Excel: 0.25 = 25%
- Hollow ใช้ group-aware calibration: non-overripe ต่ำ, overripe scaling จาก hollow_raw


## v22.7 TEST5 Tuned
- Tuned from TEST5 field CSV 60 samples
- New ripeness decision tree using f_mean, low_ratio, flatness, f_peak, firmness
- Linear field correction for Brix/Firmness/Juice
- Hollow offset +0.65 after v22.6 correction


## v22.8 Three-stage Ripeness
- ตัดระดับ “ดิบ” ออกจากผลลัพธ์หลัก
- แสดงระดับสุก 3 ช่วง: ใกล้สุก / สุกพอดี / สุกเกิน
- ปรับคำหวาน: หวานอ่อน→หวาน, หวานดี→หวานมาก, หวานที่สุด→หวานเจี๊ยบ
- เปลี่ยนคำสุกมากเป็นสุกเกิน
- Tuned from TEST6 3-class field data

## v22.9 Ripeness Color Tune
- เปลี่ยนสีระดับ “สุกเกิน” เป็นสีเทา
- ปรับ logic ความสุก 3 ช่วงให้มี buffer zone
- ใช้ low_ratio, high_ratio, Brix, Firmness, Juice, Hollow ร่วมกัน

## v24.1 Boundary Hint
- เพิ่มโซนก้ำกึ่ง ใกล้สุก ↔ สุกพอดี
- AI Voice:
  "แตงโมลูกนี้เริ่มเข้าสู่ช่วงสุกพอดี"
- ไม่เพิ่มระดับใหม่ใน UI
- แสดงคำแนะนำเฉพาะกรณี overlap

## v24.2 Hollow Warning Voice
- ถ้าไม่มีโพรง AI Voice จะไม่พูดเรื่องโพรง
- ถ้าเริ่มมีโพรง จะมีเสียงเตือน 1 ครั้ง แล้วพูด: "คำเตือน แตงโมลูกนี้เริ่มมีโพรงภายใน"
- ถ้ามีโพรงมาก จะมีเสียงเตือน 2 ครั้ง แล้วพูด: "คำเตือน แตงโมลูกนี้มีโพรงภายในค่อนข้างมาก"
- ตรวจ syntax ผ่าน node --check

## v24.3 Hollow Warning Fix
- แก้ระบบคำเตือนโพรงให้ตรวจจากค่า hollowPercent โดยตรง
- เริ่มมีโพรง: hollowPercent >= 7
- มีโพรงมาก: hollowPercent >= 18
- เพิ่มปุ่ม "ทดสอบเสียงเตือน" ใกล้ Debug
- เพิ่มระดับเสียง beep ให้ชัดขึ้น

## v24.6 F-mean Ripeness Only
- ใช้ฐาน v24.3 เดิม
- แก้เฉพาะ logic ระดับการสุก
- ใช้ f_mean เป็นแกนหลัก ตามแนวคิดเวอร์ชันแรก
- Threshold หลัก:
  - f_mean < 400 Hz = สุกเกิน
  - 400–500 Hz = สุกพอดี
  - 500–590 Hz = ใกล้สุก
  - >590 Hz = ใกล้สุก/ดิบฝั่งยังไม่พร้อม
- ไม่แตะ Brix / Firmness / Juice / Hollow
- ไม่แตะระบบรับเสียง / FFT / tap detection / AI Voice / Export

## v24.6.1 F-mean Label Fix
- แยกคำแสดงผลให้ชัดเจนระหว่าง f_peak และ f_mean
- ระบุว่า f_mean คือค่าที่ใช้จำแนกระดับการสุก
- ไม่แก้ระบบรับเสียง / FFT / Brix / Firmness / Juice / Hollow

## v24.7 F-mean Threshold Tuned
- ปรับเฉพาะระดับการสุก
- ใช้ f_mean เป็นแกนหลักตามแนวคิดเวอร์ชันแรก
- Threshold:
  - <370 Hz = สุกเกิน
  - 370–410 Hz = ก้ำกึ่ง สุกเกิน/สุกพอดี ใช้โพรง ความแน่น low_ratio ช่วยตัดสิน
  - 410–500 Hz = สุกพอดี
  - 500–525 Hz = ก้ำกึ่ง สุกพอดี/ใกล้สุก ใช้ Brix Juice Hollow Firmness ช่วยตัดสิน
  - 525–650 Hz = ใกล้สุก
  - >650 Hz = ใกล้สุก/ยังไม่พร้อม
- ไม่แตะระบบรับเสียง, FFT, Brix, Firmness, Juice, Hollow

## v24.7.1 Requested Patch
- ใช้ฐาน v24.7
- รวม A+B เป็นสุกเกิน/อาจเน่า/มักมีโพรง
- รวม D+E เป็นใกล้สุก/ดิบ
- ถ้า f_mean สูงมากกว่า 650 Hz ให้เตือนว่า “มีแนวโน้มยังดิบมาก”
- ลบข้อความ “AI Voice Summary อ่านสรุปผลแบบภาษาคน ไม่อ่านตัวเลข”
- ลบข้อความ “วิเคราะห์เสียงช่วง 50-1000Hz”
- ส่วนอื่นคงเดิม ไม่แตะระบบรับเสียง/FFT/Brix/Firmness/Juice/Hollow/Export
