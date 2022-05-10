# QuanLyChiTieu
Đồ án nhóm Cuối kì 2
from tkinter import *
from tkinter.ttk import *
from tkinter import ttk

class QuanLyChiTieu:
    def __init__(self):
        self.Dic={}
        self.TongCTNgay=0
        self.TongCTThang=0
        self.CTNgay=[]
        self.CTThang=[]
        # PHẦN GIAO DIỆN
        self.window= Tk()
        self.window.title("Quản lý chi tiêu")
        self.window.geometry("850x420")
        self.window.resizable(width=False,height=False)

        # Phần tiêu đề
        self.Tieude = Label(master=self.window,text="QUẢN LÝ CHI TIÊU",font=("#9Slide03 Nokia Regular",20,"bold"))
        self.Tieude.place(x=220,y=8)

        # Danh sách tháng
        self.Thang = Label(master=self.window,text="Tháng 5",font=("Arial",15,"bold"))
        # self.DSThang= Combobox(self.window,state="readonly")
        # self.DSThang['value']=("Tháng 1","Tháng 2","Tháng 3",
        #                 "Tháng 4","Tháng 5","Tháng 6",
        #                 "Tháng 7","Tháng 8","Tháng 9",
        #                 "Tháng 10","Tháng 11","Tháng 12")
        self.Thang.place(x= 110,y=75)
        # self.DSThang.place(x=120,y=82)

        # Danh sách ngày
        self.Ngay= Label(master=self.window,text="Ngày",font=("Arial",10,"bold"))
        self.DSNgay= Combobox(self.window,state="readonly")
        self.DSNgay['value']=("1","2","3","4","5","6","7","8","9","10",
                        "11","12","13","14","15","16","17","18","19","20",
                        "21","22","23","24","25","26","27","28","29","30")
        self.Ngay.place(x=40,y=110)
        self.DSNgay.place(x=120,y=112)

        # Số dư
        self.Sodu = Label(master=self.window,text="Số dư TK",font=("Arial",10,"bold"))
        self.SoduEntry = Entry(master=self.window,width=23)
        self.Sodu.place(x=40,y=140)
        self.SoduEntry.place(x=120,y=142)

        # Phần danh sách chi tiêu
        self.Chitieu= Label(master=self.window,text="Chi tiêu",font=("Arial",10,"bold"))
        self.DSChiTieu = Combobox(self.window, state="readonly")
        self.DSChiTieu['value']=("Shopee","Ăn uống","Mua sắm","Áo quần","Mỹ phẩm","Đi lại","Du lịch","Đổ xăng","Điện thoại","Khác")

        self.Chitieu.place(x=40,y=170)
        self.DSChiTieu.place(x=120,y=172)

        # Số tiền chi tiêu cho sản phẩm
        self.SoTien= Label(master=self.window,text="Số tiền",font=("Arial",10,"bold"))
        self.SoTienEntry= Entry(master=self.window,width=23)

        self.SoTien.place(x=40,y=200)
        self.SoTienEntry.place(x=120,y=202)

        # Các nút bấm
        self.Them = Button(master=self.window, text="Thêm",width=10,command=self.Them_chi_tieu)
        self.CapNhat = Button(master=self.window,text="Cập nhật",width=10,command=self.Cap_nhat_chi_tieu)
        self.Xoa = Button(master=self.window,text="Xóa",width=10,command=self.Xoa_chi_tieu)
        self.Lammoi = Button(master=self.window,text="Làm mới",width=15,command=self.Lam_moi)
        self.ThongKeCTNgay= Button(master=self.window,text="Thống kê chi tiêu ngày",width=37,command=self.Thong_ke_CT_ngay)
        self.ThongKeCTThang= Button(master=self.window,text="Thống kê chi tiêu tháng",width=37)

        self.Them.place(x=40, y =250)
        self.CapNhat.place(x=120,y=250)
        self.Xoa.place(x=200,y=250)
        self.Lammoi.place(x=280, y =250)
        self.ThongKeCTNgay.place(x=40,y = 280)
        self.ThongKeCTThang.place(x=40,y=310)

        # Tổng chi tiêu ngày
        self.TongChiTieuNgay= Label(master=self.window,text="TỔNG CHI TIÊU NGÀY: ",font=("Arial",13,"bold"))
        self.TongChiTieuNgayButton= Button(master=self.window,text="Tổng CT ngày",width=15,command=self.Tong_chi_tieu_ngay)

        self.TongChiTieuNgay.place(x=40,y=343)
        self.TongChiTieuNgayButton.place(x=280, y=280)

        # Tổng chi tiêu tháng
        self.TongChiTieuThang = Label(master=self.window,text="TỔNG CHI TIÊU THÁNG: ",font=("Arial",13,"bold"))
        self.TongChiTieuThangButton = Button(master=self.window,text="Tổng CT tháng",width=15,command=self.Tong_chi_tieu_thang)

        self.TongChiTieuThang.place(x=40,y=370)
        self.TongChiTieuThangButton.place(x=280,y=310)

        # Phần thống kê chi tiêu trong ngày
        self.DSCTNgay= Label(master=self.window,text="Danh sách chi tiêu trong ngày : ",font=("Arial",10,"bold"))
        self.DSChiTieuTrongNgay = ttk.Treeview(master=self.window,columns=("Name","Price","Số dư TK"), height=5)
        self.DSChiTieuTrongNgay.bind('<ButtonRelease-1>',self.SelectItemNgay)

        self.DSChiTieuTrongNgay.heading("Name",text="Name")
        self.DSChiTieuTrongNgay.heading("Price",text="Price")
        self.DSChiTieuTrongNgay.heading("Số dư TK",text="Số dư TK")
        self.DSChiTieuTrongNgay.column("#0", stretch=NO, minwidth=0, width=0)

        self.DSChiTieuTrongNgay.column("Name",stretch=NO, minwidth=0, width=134)
        self.DSChiTieuTrongNgay.column("Price", stretch=NO, minwidth=0, width=134)
        self.DSChiTieuTrongNgay.column("Số dư TK",stretch=NO, minwidth=0,width=134)

        self.DSCTNgay.place(x=400,y=60)
        self.DSChiTieuTrongNgay.place(x=400,y=90)

        # Danh sách các chi tiêu trong tháng
        self.DSThang= Label(master=self.window,text="Danh sách chi tiêu trong tháng: ",font=("Arial",10,"bold"))
        self.DSChiTieuTrongThang = ttk.Treeview(master=self.window,columns=("Date","Price"), height=5)
        self.DSChiTieuTrongThang.heading("Date",text="Date")
        self.DSChiTieuTrongThang.heading("Price",text="Price")

        self.DSChiTieuTrongThang.column("Date",stretch=NO,minwidth=0,width=201)
        self.DSChiTieuTrongThang.column("Price",stretch=NO,minwidth=0,width=201)
        self.DSChiTieuTrongThang.column("#0", stretch=NO, minwidth=0, width=0)

        self.DSThang.place(x=400,y=230)
        self.DSChiTieuTrongThang.place(x=400,y=260)

    def Them_chi_tieu(self):
        SoTien= self.SoTienEntry.get()
        ChiTieu= self.DSChiTieu.get()
        SoDu = self.SoduEntry.get()
        self.DSChiTieuTrongNgay.insert('', 'end', values=(ChiTieu,SoTien,SoDu))
        self.CTNgay.append([ChiTieu,SoTien,SoDu])

    def Cap_nhat_chi_tieu(self):
        selected = self.DSChiTieuTrongNgay.focus()
        self.DSChiTieuTrongNgay.item(selected,values=(self.DSChiTieu.get(),self.SoTienEntry.get(),self.SoduEntry.get()))

    def Xoa_chi_tieu(self):
        selected = self.DSChiTieuTrongNgay.focus()
        self.DSChiTieuTrongNgay.delete(selected)

        selected = self.DSChiTieuTrongThang.focus()
        self.DSChiTieuTrongThang.delete(selected)

    def Lam_moi(self):
        self.SoduEntry.delete(0,END)
        self.DSChiTieu.set("")
        self.SoTienEntry.delete(0,END)

    def Thong_ke_CT_ngay(self):
        Ngay=self.DSNgay.get()
        self.DSChiTieuTrongThang.insert('','end',values=("Ngày "+Ngay,self.Tong_chi_tieu_ngay()))
        self.CTThang.append([Ngay,self.Tong_chi_tieu_ngay()])

    def Tong_chi_tieu_ngay(self):
        self.TongCTNgay = 0
        for i in range(len(self.CTNgay)):
            self.TongCTNgay += float(self.CTNgay[i][1])
        self.TongChiTieuNgay.configure(text=f"TỔNG CHI TIÊU NGÀY: {round(self.TongCTNgay)} VNĐ")
        return self.TongCTNgay

    def Tong_chi_tieu_thang(self):
        self.TongCTThang =0
        for i in range(len(self.CTThang)):
            self.TongCTThang += float(self.CTThang[i][1])
        self.TongChiTieuThang.configure(text=f"TỔNG CHI TIÊU THÁNG: {round(self.TongCTThang)} VNĐ")
        return self.TongCTThang

    def SelectItemNgay(self,a):
        selected = self.DSChiTieuTrongNgay.focus()
        Lis = self.DSChiTieuTrongNgay.item(selected)['values']

        self.SoduEntry.delete(0,END)
        self.SoduEntry.insert(0,Lis[2])

        self.SoTienEntry.delete(0,END)
        self.SoTienEntry.insert(0,Lis[1])

        for i in range(len(self.DSChiTieu['value'])):
            if self.DSChiTieu['value'][i] == Lis[0]:
                self.DSChiTieu.set(Lis[0])

if __name__ == "__main__":
    app = QuanLyChiTieu()
    app.window.mainloop()

