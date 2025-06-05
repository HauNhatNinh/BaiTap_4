# Hầu Nhật Ninh - K225480106077 - K58KTP.K01
## BaiTap_4
Tạo csdl cho hệ thống TKB

Yêu cầu bài toán:
 - Tạo csdl cho hệ thống TKB (đã nghe giảng, đã xem cách làm)
 - Nguồn dữ liệu: TMS.tnut.edu.vn
 - Tạo các bảng tuỳ ý (3nf)
 - Tạo được query truy vấn ra thông tin gồm 4 cột: họ tên gv, môn dạy, giờ vào lớp, giờ ra.
   Trả lời câu hỏi: trong khoảng thời gian từ datetime1 tới datetime2 thì có những gv nào đang bận giảng dạy.

Các bước thực hiện:
1. Tạo github repo mới: đặt tên tuỳ ý (có liên quan đến bài tập này)
2. tạo file readme.md, edit online nó:
   paste những ảnh chụp màn hình gõ text mô tả cho ảnh đó
   
Gợi ý:
  - Sử dung tms => dữ liệu thô => tiền xử lý => dữ liệu như ý (3nf)
  - Tạo các bảng với struct phù hợp
  - Insert nhiều rows từ excel vào cửa sổ edit dữ liệu 1 table (quan sát thì sẽ làm đc)

deadline: 15/4/2025

# Bài Làm 
## 1. Tạo csdl cho hệ thống TKB
 - Truy cập vào [TMS.tnut.edu.vn](https://tms.tnut.edu.vn/tkb/lop.html) để lấy dữ liệu, ở đây ta sẽ lấy ví dụ là thời khóa biểu Tuần: 33 (14/04/2025 → 20/04/2025) của lớp K58KTP

![Screenshot (30)](https://github.com/user-attachments/assets/cbed0435-566d-4380-8185-48869b86b020)

 - Ngoài ra ta cũng phải lấy dữ liệu từ bảng thời gian biểu giảng dạy này nữa

![image](https://github.com/user-attachments/assets/e5c31d51-8b9e-44b3-abf4-faa3236e8fd7)

 - Tạo database có tên là __TKB-K58KTP.K01__, sau đó tiến hành tạo các bảng có tên là __GiangVien, Lop, MonHoc, PhongHoc, TietHoc, LichDay__
  
![Screenshot (32)](https://github.com/user-attachments/assets/a553d2e8-f78a-41b0-ba21-03f0633cc2af)
 
![Screenshot (25)](https://github.com/user-attachments/assets/ba32b8a6-5c19-42be-a0eb-96674cf763fb)

![Screenshot (24)](https://github.com/user-attachments/assets/fea8ddf1-13d7-4259-b764-5d3959e0263d)
 
 - Các bảng khác làm tương tự, lưu ý các bảng tạo ra phải đạt chuẩn 3NF. Chuẩn 3NF (*Third Normal Form*) là một cấp độ chuẩn hóa cơ sở dữ liệu nhằm loại bỏ sự dư thừa dữ liệu và đảm bảo tính toàn vẹn.
    - Ở Bảng __GiangVien__: Đạt 3NF vì mọi cột đều phụ thuộc vào khóa chính __id__
    - Bảng __MonHoc__: Đạt 3NF vì không có phụ thuộc bắc cầu
    - Bảng __Lop__: Đạt 3NF vì là bảng đơn giản, không dư thừa
    - Bảng __PhongHoc__: Đạt 3NF vì không dư thừa
    - Bảng __TietHoc__: Đạt chuẩn 3NF vì không có cột nào phụ thuộc cột không khóa khác
    - Bảng __LichDay__: Đạt 3NF vì các cột đều mô tả thuộc tính của ca giảng dạy, tất cả phụ thuộc vào cột __id__ ở trong bảng, không có cột nào dư thừa  
 - Sau đó tạo mối liên kết giữa các bảng (FK) và tiến hành nhập dữ liệu từ thời khóa biểu Tuần: 33 (14/04/2025 → 20/04/2025) của lớp K58KTP.K01 và bảng thời gian biểu giảng dạy ở trên cho từng bảng.

![image](https://github.com/user-attachments/assets/51e0fec9-fbf4-4a3d-a26e-fd2eb736a474)
   
 - Khi hoàn thành các bước trên ta có được sơ đồ sau.

![Screenshot (29)](https://github.com/user-attachments/assets/accf9d27-a17c-4e19-a6b0-c082ce88b8d4)
   
 - Lúc này ta đã có thể tạo được query truy vấn ra thông tin gồm 4 cột: *họ tên gv, môn dạy, giờ vào lớp, giờ ra* theo yêu cầu của đề bài. Nhưng như ta có thể thấy, thông tin truy vấn ra như vậy là còn khá chung chung, người khác nhìn vào chưa chắc đã nắm rõ được các thông tin.
 - Ta sẽ thay đổi đôi chút bằng cách thêm các cột như __Thứ, Ngày, Số tiết__ vào vào viết lệnh sao cho ta có thể truy xuất được cụ thể một giảng viên nhất định bằng việc nhập __id__ của giảng viên đó

![Screenshot (31)](https://github.com/user-attachments/assets/f5e00627-6c9e-4ed6-9dee-9a00ece4b07a)
   
 ```sql
SELECT 
    gv.ten_gv AS [Họ tên GV],
    mh.ten_mon AS [Môn dạy],
    CASE 
        WHEN ld.thu = 2 THEN 'T2'
        WHEN ld.thu = 3 THEN 'T3'
        WHEN ld.thu = 4 THEN 'T4'
        WHEN ld.thu = 5 THEN 'T5'
        WHEN ld.thu = 6 THEN 'T6'
        WHEN ld.thu = 7 THEN 'T7'
        WHEN ld.thu = 8 THEN 'CN'
        ELSE 'N/A'
    END AS [Thứ],
    FORMAT(ld.ngay, 'dd/MM/yyyy') AS [Ngày],
    ld.so_tiet AS [Số tiết],
    MIN(t.gio_vao) AS [Giờ vào lớp],
    MAX(t.gio_ra) AS [Giờ ra lớp]
FROM 
    LichDay ld
    INNER JOIN GiangVien gv ON ld.giang_vien_id = gv.id
    INNER JOIN MonHoc mh ON ld.mon_hoc_id = mh.id
    INNER JOIN TietHoc t ON t.tiet BETWEEN ld.tiet_bat_dau AND (ld.tiet_bat_dau + ld.so_tiet - 1)
WHERE 
    ld.giang_vien_id = 4  -- Thay bằng ID GV bạn cần tìm
GROUP BY
    gv.ten_gv,
    mh.ten_mon,
    ld.thu,
    ld.ngay,
    ld.so_tiet,
    ld.tiet_bat_dau
ORDER BY 
    ld.ngay, MIN(t.gio_vao);






 
