# The goal of this notebook is to create and conduct simple query, I use DBeaver on Postgre environment
![image](https://user-images.githubusercontent.com/96459463/166402579-045105c3-a276-4c6f-acf5-e9d27e96366a.png)
 ![image](https://user-images.githubusercontent.com/96459463/166422237-8792b758-9245-4286-8e27-fed8787a48de.png)
![image](https://user-images.githubusercontent.com/96459463/166422252-4c00d683-3cca-4003-b666-4efb7a7c696d.png)
![image](https://user-images.githubusercontent.com/96459463/166422265-6cab41a8-f9b2-4e83-8670-2946cb2ead6c.png)

 --create database
create database QuanLyDeAnCongTy

--create tables NHANVIEN & PHONGBAN
create table NHANVIEN
(
HONV varchar(15),
TENLOT varchar(15),
TENNV varchar(15),
MANV char(9) NOT NULL,
NGSINH timestamp,
DCHI varchar(30),
PHAI varchar(3),
LUONG float,
MA_NQL char(9),
PHG int,
PRIMARY KEY (MANV)
)

create table PHONGBAN
(
TENPHG varchar(15),
MAPHG int NOT NULL,
TRPHG char(9),
NG_NHANCHUC timestamp,
PRIMARY KEY (MAPHG)
)
--create tables
Create table diadiem_phg
(
Maphg int,
Diadiem varchar(15),
CONSTRAINT PK_diadiem_phg PRIMARY KEY (maphg,diadiem),
CONSTRAINT FK_diadiem_phg_phongban FOREIGN KEY (maphg) REFERENCES phongban(Maphg)
)
Create table dean
(
Tenda varchar(15),
Mada int,
Ddiem_da varchar(15),
phong int,
CONSTRAINT PK_dean PRIMARY KEY (mada),
CONSTRAINT FK_dean_phongban FOREIGN KEY (phong) REFERENCES phongban(Maphg)
)
Create table congviec
(
Mada int,
Stt int,
Ten_cong_viec varchar(50),
CONSTRAINT PK_congviec PRIMARY KEY (mada,stt),
CONSTRAINT FK_congviec_dean FOREIGN KEY (mada) REFERENCES dean(Mada)
)
Create table phancong
(
Ma_nvien char(9),
Mada int,
Stt int,
Thoigian float,
CONSTRAINT PK_phancong PRIMARY KEY (ma_nvien,mada,stt),
CONSTRAINT FK_phancong_NHANVIEN FOREIGN KEY (ma_nvien) REFERENCES NHANVIEN(Manv),
CONSTRAINT FK_phancong_congviec FOREIGN KEY (mada,stt) REFERENCES congviec(Mada,stt)
)
Create table thannhan
(
Ma_nvien char(9),
Tentn varchar(15),
phai char(3),
ngsinh timestamp,
quanhe char(15),
CONSTRAINT PK_thannhan PRIMARY KEY (ma_nvien,tentn),
CONSTRAINT FK_thanhnhan_NHANVIEN FOREIGN KEY (ma_nvien) REFERENCES NHANVIEN(Manv)
)

--add contraints (check PHAI)
ALTER TABLE NHANVIEN
ADD CONSTRAINT C_PHAI
CHECK (phai IN ('Nam', 'Nữ'))

--add contraints (foreign key)
ALTER TABLE nhanvien
ADD CONSTRAINT FK_nhanvien_phongban FOREIGN KEY (phg) REFERENCES phongban(Maphg)

ALTER TABLE phongban
ADD constraint FK_PHONGBAN_NHANVIEN FOREIGN KEY (Trphg) REFERENCES NHANVIEN(Manv)

--insert into NHANVIEN
INSERT INTO nhanvien(honv,tenlot,tennv,manv,ngsinh,dchi,phai,luong,ma_nql,phg )
values ('Đinh', 'Bá', 'Tiên', '9', '02/11/1960', '119 Cống Quỳnh, Tp HCM', 'Nam', 30000, '5', null),
('Nguyễn', 'Thanh', 'Tùng', '5', '08/20/1962', '222 Nguyễn Văn Cừ, Tp HCM', 'Nam', 40000, '6', null),
('Bùi', 'Ngọc', 'Hằng', '7', '03/11/1954', '332 Nguyễn Thái Học, Tp HCM', 'Nam', 25000, '1', null),
('Lê', 'Quỳnh', 'Như', '1', '02/01/1967', '291 Hồ Văn Huê, Tp HCM', 'Nữ', 43000, '6', null),
('Nguyễn', 'Mạnh', 'Hùng', '4', '03/04/1967', '95 Bà Rịa, Vũng Tàu', 'Nam', 38000, '5', null),
('Trần', 'Thanh', 'Tâm', '3', '05/04/1957', '34 Mai Thị Lự, Tp HCM', 'Nam', 25000, '5', null),
('Trần', 'Hồng', 'Quang', '8', '09/01/1967', '80 Lê Hồng Phong, Tp HCM', 'Nam', 25000, '1', null),
('Phạm', 'Văn', 'Vinh', '6', '01/01/1965', '45 Trưng Vương, Hà Nội', 'Nữ', 55000, null, null)

--insert into PHONGBAN
INSERT INTO phongban (TENPHG,MAPHG,TRPHG,NG_NHANCHUC)
VALUES
(N'Nghiên cứu', '5',null , '05/22/1978 '),
(N'Điều hành', '4',null, '01/01/1985 '),
(N'Quản lý', '1',null, '06/19/1971')

--insert into DEAN
INSERT INTO dean(TENDA,MADA,DDIEM_DA,PHONG)
VALUES
('San pham X', 1, 'Vũng Tàu', '5 '),
('San pham Y', 2, 'Nha Trang', '5 '),
('San pham Z', 3, 'TP HCM', '5 '),
('Tin hoc hoa', 10, 'Hà Nội', '4 '),
('Cap quang', 20, 'TP HCM', '1 '),
('Dao tao', 30, 'Hà Nội', '4')
--insert into DIADIEM_PHG
INSERT into diadiem_phg (MAPHG,DIADIEM)
VALUES
('1', 'TP HCM '),
('4', 'Hà Nội '),
('5', 'TAU '),
('5', 'NHA TRANG '),
('5', 'TP HCM')
--insert into CONGVIEC
INSERT INTO congviec (mada,stt,ten_cong_viec)
VALUES
('1', '1', 'Thiet ke san pham X '),
('1', '2', 'Thu nghiem san pham X '),
('2', '1', 'San xuat san pham Y '),
('2', '2', 'Quang cao san pham Y '),
('3', '1', 'Khuyen mai san pham Z '),
('10', '1', 'Tin hoc hoa nhan su tien luong '),
('10', '2', 'Tin hoc hoa phong Kinh doanh '),
('20', '1', 'Lap dat cap quang '),
('30', '1', 'Dao tao nhan vien Marketing '),
('30','2','Dao tao nhan vien thiet ke');
--insert into PHANCONG
INSERT INTO phancong (MA_NVIEN,MADA,STT,THOIGIAN)
VALUES
('9', 1, 1, 32),
('9', 2, 2, 8),
('4', 3, 1, 40),
('3', 1, 2, 20.0 ),
('3', 2, 1, 20.0),
('8', 10, 1, 35),
('8', 30 , 2, 5),
('1', 30, 1, 20),
('1', 20, 1, 15)

--insert into THANNHAN
INSERT INTO thannhan (MA_NVIEN,TENTN,PHAI,NGSINH,QUANHE)
VALUES
('5', 'Trinh', 'Nữ', '04/05/1976', 'Con gái '),
('5', 'Khang', 'Nam', '10/25/1973', 'Con trai '),
('5', 'Phương', 'Nữ', '05/03/1948', 'Vợ chồng '),
('1', 'Minh', 'Nam', '02/29/1932', 'Vợ chồng '),
('9', 'Tiến', 'Nam', '01/01/1978', 'Con trai '),
('9', 'Châu', 'Nữ', '12/30/1978', 'Con gái '),
('9', 'Phương', 'Nữ', '05/05/1957', 'Vợ chồng')
--update nhanvien
UPDATE nhanvien
SET phg = ('5')
where manv=('5')
--Q1
select * from nhanvien where phg='4'
--Q2
select * from nhanvien where luong >30000
--Q3
select * from nhanvien where (luong > 25000 and phg='4') or (luong > 30000 and phg='5')
--Q4
SELECT CONCAT(honv, tenlot,tennv) AS hoten FROM nhanvien where dchi like '%Tp HCM'
--Q5
SELECT CONCAT(honv, tenlot,tennv) AS hoten FROM nhanvien where honv like 'N%'
--Q6
select ngsinh,dchi from nhanvien where CONCAT(honv, tenlot,tennv)='dinh ba tien'
--Q7
SELECT CONCAT(honv, tenlot,tennv) FROM nhanvien
WHERE extract(year from NGSINH) BETWEEN 1960 AND 1965;
--Q8
SELECT CONCAT(honv, tenlot,tennv),extract(year from NGSINH)FROM nhanvien
--Q9
SELECT CONCAT(honv, tenlot,tennv),2022-(extract(year from NGSINH)) FROM nhanvien
