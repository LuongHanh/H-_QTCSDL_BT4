# Bài tập 04 của sv: K225480106013 - Lường Văn Hạnh - Môn Hệ quản trị csdl
# Deadline: 15/4/2025
# Đề Bài
## Yêu cầu bài toán:
 - Tạo csdl cho hệ thống TKB (đã nghe giảng, đã xem cách làm)
 - Nguồn dữ liệu: TMS.tnut.edu.vn
 - Tạo các bảng tuỳ ý (3nf)
 - Tạo được query truy vấn ra thông tin gồm 4 cột: họ tên gv, môn dạy, giờ vào lớp, giờ ra.
   trả lời câu hỏi: trong khoảng thời gian từ datetime1 tới datetime2 thì có những gv nào đang bận giảng dạy.

## Các bước thực hiện:
1. Tạo github repo mới: đặt tên tuỳ ý (có liên quan đến bài tập này)
2. tạo file readme.md, edit online nó:
   paste những ảnh chụp màn hình
   gõ text mô tả cho ảnh đó

## Gợi ý:
  sử dung tms => dữ liệu thô => tiền xử lý => dữ liệu như ý (3nf)
  tạo các bảng với struct phù hợp
  insert nhiều rows từ excel vào cửa sổ edit dữ liệu 1 table (quan sát thì sẽ làm đc)

# Bài làm
## Tạo csdl cho hệ thống TKB 
Tạo db có tên ThoiKhoaBieu:  
![Ảnh chụp màn hình 2025-04-15 223358](https://github.com/user-attachments/assets/578cb2e4-cfb3-4b58-a854-a1346679be56)
![Ảnh chụp màn hình 2025-04-15 223528](https://github.com/user-attachments/assets/634651fc-fced-4cef-b115-9360282e9a41)

Tạo các bảng bằng lệnh SQL:
Cách 1 : Tạo bằng Lệnh SQL:
![Ảnh chụp màn hình 2025-04-15 223457](https://github.com/user-attachments/assets/02afdf7d-470b-4495-8bc1-fc6fe1db3465)

Code SQL:
```sql
	CREATE TABLE GiaoVien (
		MaGV VARCHAR(6) PRIMARY KEY,
		HoTen NVARCHAR(30)
	);

	CREATE TABLE MonHoc (
		MaMH VARCHAR(7) PRIMARY KEY,
		TenMH NVARCHAR(100)
	);

	CREATE TABLE LopHP (
		MaLHP VARCHAR(11) PRIMARY KEY,
		TenLHP NVARCHAR(100),
		MaGV VARCHAR(6) NOT NULL,
		MaMH VARCHAR(7) NOT NULL,
		FOREIGN KEY (MaGV) REFERENCES GiaoVien(MaGV),
		FOREIGN KEY (MaMH) REFERENCES MonHoc(MaMH)
	);

	CREATE TABLE ThoiKhoaBieu(
		ID INT PRIMARY KEY IDENTITY,
		MaLHP VARCHAR(11) NOT NULL,
		Thu INT,                 
		TietBatDau INT,           
		SoTiet INT,               
		TuanHoc INT,   
		Phong VARCHAR(10),
		FOREIGN KEY (MaLHP) REFERENCES LopHP(MaLHP)
	);
```
Cách 2: Tạo bằng UI:
![Ảnh chụp màn hình 2025-04-15 223834](https://github.com/user-attachments/assets/feac2465-02b8-45fd-88ef-dd4b62669de6)

![Ảnh chụp màn hình 2025-04-15 223842](https://github.com/user-attachments/assets/da51d47c-bd9c-4737-8734-0b42fb2dd6db)

![Ảnh chụp màn hình 2025-04-15 223850](https://github.com/user-attachments/assets/b8ca25e7-3fb3-4dff-ab4b-673eae3d8c57)

![Ảnh chụp màn hình 2025-04-15 223856](https://github.com/user-attachments/assets/b83e90be-d154-4bde-b91e-5826c50d52c3)

## Nguồn dữ liệu: TMS.tnut.edu.vn - Tạo các bảng tuỳ ý (3nf)
Copy dữ liệu trên trang TMS.tnut.edu.vn rồi dùng excel để xử lý:
![Ảnh chụp màn hình 2025-04-15 223716](https://github.com/user-attachments/assets/99712642-0802-47cb-975d-49fc190839f4)

Sau khi xử lý xong thì paste dữ liệu vào các bảng tương ứng:

![Ảnh chụp màn hình 2025-04-15 223742](https://github.com/user-attachments/assets/95f790e4-1e02-46b2-bd47-e1af29393536)

![Ảnh chụp màn hình 2025-04-15 223750](https://github.com/user-attachments/assets/123f0527-f3d1-4025-85a6-4012f1c81ec1)

![Ảnh chụp màn hình 2025-04-15 223758](https://github.com/user-attachments/assets/75590239-7f8d-41a1-a028-0932f299f0d9)

![Ảnh chụp màn hình 2025-04-15 223804](https://github.com/user-attachments/assets/9801282f-fbf0-41d5-a3f1-34d673f346e8)

## Tạo được query truy vấn ra thông tin gồm 4 cột: họ tên gv, môn dạy, giờ vào lớp, giờ ra.
Các lệnh truy vấn: Sử dụng bảng tạm đảm bảo sự linh hoạt
```sql
DECLARE @datetime1 DATETIME = '2025-04-15 07:30:00';
DECLARE @datetime2 DATETIME = '2025-04-15 09:10:00';

WITH TietHoc AS (	--Bang tạm
    SELECT * FROM (VALUES 
        (1, CAST('06:30:00' AS TIME), CAST('07:45:00' AS TIME)), -- ép kiểu cho các cột
        (2, '07:55:00', '09:10:00'), -- từ dòng này SQL tự hieu là phai ếp kiểu
        (3, '09:20:00', '10:35:00'),
        (4, '10:45:00', '12:00:00'),
        (5, '12:30:00', '13:45:00'),
        (6, '13:55:00', '15:10:00'),
        (7, '15:20:00', '16:35:00'),
        (8, '16:45:00', '18:00:00'),
        (9, '18:10:00', '19:25:00'),
        (10, '19:35:00', '20:50:00')
    ) AS T(SoTiet, GioBatDau, GioKetThuc)
),
TKB_HopNhat AS (
    SELECT
        gv.HoTen,
        mh.TenMH,
        -- Ghép ngày + giờ
        CAST(CAST(@datetime1 AS DATE) AS DATETIME) + CAST(th1.GioBatDau AS DATETIME) AS GioVao,
        CAST(CAST(@datetime1 AS DATE) AS DATETIME) + CAST(th2.GioKetThuc AS DATETIME) AS GioRa
    FROM TKB tkb
    JOIN LopHP lhp ON tkb.MaLHP = lhp.MaLHP
    JOIN GiaoVien gv ON lhp.MaGV = gv.MaGV
    JOIN MonHoc mh ON lhp.MaMH = mh.MaMH
    JOIN TietHoc th1 ON tkb.TietBatDau = th1.SoTiet
    JOIN TietHoc th2 ON (tkb.TietBatDau + tkb.SoTiet - 1) = th2.SoTiet
    WHERE tkb.Thu = DATEPART(WEEKDAY, @datetime1)
)
SELECT HoTen [Họ và tên], TenMH [Tên môn học], GioVao [Giờ vào], GioRa [Giờ ra]
FROM TKB_HopNhat
WHERE NOT (
    GioRa <= @datetime1 OR GioVao >= @datetime2
);
```
Sau khi chạy các lênh trên:
![Ảnh chụp màn hình 2025-04-15 223955](https://github.com/user-attachments/assets/7d2325b9-4ca4-4083-b18a-1d155ebedd2b)
