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
CHECK (phai IN ('Nam', 'N???'))

--add contraints (foreign key)
ALTER TABLE nhanvien
ADD CONSTRAINT FK_nhanvien_phongban FOREIGN KEY (phg) REFERENCES phongban(Maphg)

ALTER TABLE phongban
ADD constraint FK_PHONGBAN_NHANVIEN FOREIGN KEY (Trphg) REFERENCES NHANVIEN(Manv)

--insert into NHANVIEN
INSERT INTO nhanvien(honv,tenlot,tennv,manv,ngsinh,dchi,phai,luong,ma_nql,phg )
values ('??inh', 'B??', 'Ti??n', '9', '02/11/1960', '119 C???ng Qu???nh, Tp HCM', 'Nam', 30000, '5', null),
('Nguy???n', 'Thanh', 'T??ng', '5', '08/20/1962', '222 Nguy???n V??n C???, Tp HCM', 'Nam', 40000, '6', null),
('B??i', 'Ng???c', 'H???ng', '7', '03/11/1954', '332 Nguy???n Th??i H???c, Tp HCM', 'Nam', 25000, '1', null),
('L??', 'Qu???nh', 'Nh??', '1', '02/01/1967', '291 H??? V??n Hu??, Tp HCM', 'N???', 43000, '6', null),
('Nguy???n', 'M???nh', 'H??ng', '4', '03/04/1967', '95 B?? R???a, V??ng T??u', 'Nam', 38000, '5', null),
('Tr???n', 'Thanh', 'T??m', '3', '05/04/1957', '34 Mai Th??? L???, Tp HCM', 'Nam', 25000, '5', null),
('Tr???n', 'H???ng', 'Quang', '8', '09/01/1967', '80 L?? H???ng Phong, Tp HCM', 'Nam', 25000, '1', null),
('Ph???m', 'V??n', 'Vinh', '6', '01/01/1965', '45 Tr??ng V????ng, H?? N???i', 'N???', 55000, null, null)

--insert into PHONGBAN
INSERT INTO phongban (TENPHG,MAPHG,TRPHG,NG_NHANCHUC)
VALUES
(N'Nghi??n c???u', '5',null , '05/22/1978 '),
(N'??i???u h??nh', '4',null, '01/01/1985 '),
(N'Qu???n l??', '1',null, '06/19/1971')

--insert into DEAN
INSERT INTO dean(TENDA,MADA,DDIEM_DA,PHONG)
VALUES
('San pham X', 1, 'V??ng T??u', '5 '),
('San pham Y', 2, 'Nha Trang', '5 '),
('San pham Z', 3, 'TP HCM', '5 '),
('Tin hoc hoa', 10, 'H?? N???i', '4 '),
('Cap quang', 20, 'TP HCM', '1 '),
('Dao tao', 30, 'H?? N???i', '4')
--insert into DIADIEM_PHG
INSERT into diadiem_phg (MAPHG,DIADIEM)
VALUES
('1', 'TP HCM '),
('4', 'H?? N???i '),
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
('5', 'Trinh', 'N???', '04/05/1976', 'Con g??i '),
('5', 'Khang', 'Nam', '10/25/1973', 'Con trai '),
('5', 'Ph????ng', 'N???', '05/03/1948', 'V??? ch???ng '),
('1', 'Minh', 'Nam', '02/29/1932', 'V??? ch???ng '),
('9', 'Ti???n', 'Nam', '01/01/1978', 'Con trai '),
('9', 'Ch??u', 'N???', '12/30/1978', 'Con g??i '),
('9', 'Ph????ng', 'N???', '05/05/1957', 'V??? ch???ng')
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
