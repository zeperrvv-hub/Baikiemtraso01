![Uploading image.png…]()

using System;

namespace AutoSpeedOOP
{
    public abstract class PhuongTien
    {
        // Private Fields
        private string _maPT;
        private string _tenHang;
        private int _namSanXuat;
        private decimal _giaGoc;

        // Properties
        public string MaPT
        {
            get { return _maPT; }
            set
            {
                if (string.IsNullOrWhiteSpace(value))
                    _maPT = "PT000";
                else
                    _maPT = value.Trim();
            }
        }

        public string TenHang
        {
            get { return _tenHang; }
            set
            {
                if (string.IsNullOrWhiteSpace(value))
                    throw new ArgumentException("Tên hãng không được để trống!");

                _tenHang = value.Trim();
            }
        }

        public int NamSanXuat
        {
            get { return _namSanXuat; }
            set
            {
                if (value < 1900 || value > DateTime.Now.Year)
                    throw new ArgumentException("Năm sản xuất không hợp lệ!");

                _namSanXuat = value;
            }
        }

        public decimal GiaGoc
        {
            get { return _giaGoc; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException("Giá gốc phải lớn hơn 0!");

                _giaGoc = value;
            }
        }

        // Constructor
        public PhuongTien(
            string maPT,
            string tenHang,
            int namSanXuat,
            decimal giaGoc)
        {
            MaPT = maPT;
            TenHang = tenHang;
            NamSanXuat = namSanXuat;
            GiaGoc = giaGoc;
        }

        // Abstract Method
        public abstract decimal TinhGiaLanBanh();

        // Virtual Method
        public virtual string GetInfo()
        {
            return $"Mã PT: {MaPT} | " +
                   $"Hãng: {TenHang} | " +
                   $"Năm SX: {NamSanXuat} | " +
                   $"Giá gốc: {GiaGoc:N0} VNĐ";
        }
    }
}
using System;

namespace AutoSpeedOOP
{
    public class OTo : PhuongTien
    {
        private int _soChoNgoi;
        private double _dungTichDongCo;

        public int SoChoNgoi
        {
            get { return _soChoNgoi; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException("Số chỗ ngồi phải lớn hơn 0!");

                _soChoNgoi = value;
            }
        }

        public double DungTichDongCo
        {
            get { return _dungTichDongCo; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException("Dung tích động cơ phải lớn hơn 0!");

                _dungTichDongCo = value;
            }
        }

        public OTo(
            string maPT,
            string tenHang,
            int namSanXuat,
            decimal giaGoc,
            int soChoNgoi,
            double dungTichDongCo)
            : base(maPT, tenHang, namSanXuat, giaGoc)
        {
            SoChoNgoi = soChoNgoi;
            DungTichDongCo = dungTichDongCo;
        }

        public override decimal TinhGiaLanBanh()
        {
            if (SoChoNgoi <= 9)
            {
                // Giá gốc + 12% lệ phí trước bạ + 30% TTĐB
                return GiaGoc
                       + GiaGoc * 0.12m
                       + GiaGoc * 0.30m;
            }
            else
            {
                // Giá gốc + 10% lệ phí trước bạ
                return GiaGoc
                       + GiaGoc * 0.10m;
            }
        }

        public override string GetInfo()
        {
            return base.GetInfo()
                   + $" | Số chỗ: {SoChoNgoi}"
                   + $" | Dung tích động cơ: {DungTichDongCo}L";
        }
    }
}
using System;

namespace AutoSpeedOOP
{
    public class XeMay : PhuongTien
    {
        private int _dungTichXylanh;

        public int DungTichXylanh
        {
            get { return _dungTichXylanh; }
            set
            {
                if (value <= 0)
                    throw new ArgumentException("Dung tích xy lanh phải lớn hơn 0!");

                _dungTichXylanh = value;
            }
        }

        public XeMay(
            string maPT,
            string tenHang,
            int namSanXuat,
            decimal giaGoc,
            int dungTichXylanh)
            : base(maPT, tenHang, namSanXuat, giaGoc)
        {
            DungTichXylanh = dungTichXylanh;
        }

        public override decimal TinhGiaLanBanh()
        {
            if (DungTichXylanh < 175)
            {
                // Giá gốc + 2% trước bạ
                return GiaGoc + GiaGoc * 0.02m;
            }
            else
            {
                // Giá gốc + 5% trước bạ
                return GiaGoc + GiaGoc * 0.05m;
            }
        }

        public override string GetInfo()
        {
            return base.GetInfo()
                   + $" | Dung tích xy lanh: {DungTichXylanh}cc";
        }
    }
}
using System;
using System.Collections.Generic;
using System.Linq;

namespace AutoSpeedOOP
{
    public class QuanLyPhuongTien
    {
        private List<PhuongTien> danhSach;

        public QuanLyPhuongTien()
        {
            danhSach = new List<PhuongTien>();
        }

        // 1. Thêm phương tiện
        public void AddPhuongTien(PhuongTien pt)
        {
            danhSach.Add(pt);
        }

        // 2. Hiển thị toàn bộ
        public void DisplayAll()
        {
            if (danhSach.Count == 0)
            {
                Console.WriteLine("Danh sách phương tiện trống!");
                return;
            }

            foreach (PhuongTien pt in danhSach)
            {
                Console.WriteLine(pt.GetInfo());

                Console.WriteLine(
                    $"Giá lăn bánh: {pt.TinhGiaLanBanh():N0} VNĐ");

                Console.WriteLine(
                    "------------------------------------------------------------");
            }
        }

        // 3. Tìm phương tiện có giá lăn bánh cao nhất
        public PhuongTien FindMaxGiaLanBanh()
        {
            if (danhSach.Count == 0)
                return null;

            PhuongTien max = danhSach[0];

            foreach (PhuongTien pt in danhSach)
            {
                if (pt.TinhGiaLanBanh() > max.TinhGiaLanBanh())
                {
                    max = pt;
                }
            }

            return max;
        }

        // 4. Tìm theo tên hãng
        public List<PhuongTien> SearchByName(string keyword)
        {
            return danhSach
                .Where(pt =>
                    pt.TenHang.Contains(
                        keyword,
                        StringComparison.OrdinalIgnoreCase))
                .ToList();
        }
    }
}
using System;
using System.Text;
using System.Collections.Generic;

namespace AutoSpeedOOP
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.OutputEncoding = Encoding.UTF8;

            Console.WriteLine("=========================================");
            Console.WriteLine(" HỆ THỐNG QUẢN LÝ PHƯƠNG TIỆN AUTOSPEED");
            Console.WriteLine("=========================================");

            QuanLyPhuongTien ql = new QuanLyPhuongTien();

            // =====================================
            // TC01 - VALIDATION NĂM SẢN XUẤT
            // =====================================

            Console.WriteLine("\n===== TC01: KIỂM TRA NĂM SẢN XUẤT =====");

            try
            {
                OTo xeLoi = new OTo(
                    "OT000",
                    "Toyota",
                    1850,
                    500000000m,
                    5,
                    1.5);

                Console.WriteLine("TC01: FAILED");
            }
            catch (ArgumentException ex)
            {
                Console.WriteLine("TC01: PASSED");
                Console.WriteLine("Lỗi: " + ex.Message);
            }


            // =====================================
            // TC02 - Ô TÔ
            // =====================================

            Console.WriteLine("\n===== TC02: GIÁ LĂN BÁNH Ô TÔ =====");

            OTo oto = new OTo(
                "OT001",
                "Toyota",
                2025,
                1000000000m,
                5,
                2.0);

            decimal giaOTo = oto.TinhGiaLanBanh();

            Console.WriteLine(oto.GetInfo());

            Console.WriteLine(
                $"Giá lăn bánh: {giaOTo:N0} VNĐ");

            // Kết quả:
            // 1,420,000,000 VNĐ


            // =====================================
            // TC03 - XE MÁY
            // =====================================

            Console.WriteLine("\n===== TC03: GIÁ LĂN BÁNH XE MÁY =====");

            XeMay xeMay = new XeMay(
                "XM001",
                "Honda",
                2025,
                50000000m,
                150);

            decimal giaXeMay =
                xeMay.TinhGiaLanBanh();

            Console.WriteLine(xeMay.GetInfo());

            Console.WriteLine(
                $"Giá lăn bánh: {giaXeMay:N0} VNĐ");

            // Kết quả:
            // 51,000,000 VNĐ


            // =====================================
            // THÊM VÀO DANH SÁCH
            // =====================================

            ql.AddPhuongTien(oto);
            ql.AddPhuongTien(xeMay);


            // Thêm thêm một vài xe để test
            OTo oto2 = new OTo(
                "OT002",
                "Hyundai",
                2024,
                800000000m,
                16,
                2.5);
XeMay xeMay2 = new XeMay(
                "XM002",
                "Yamaha",
                2024,
                100000000m,
                180);

            ql.AddPhuongTien(oto2);
            ql.AddPhuongTien(xeMay2);


            // =====================================
            // TC04 - ĐA HÌNH
            // =====================================

            Console.WriteLine("\n===== TC04: KIỂM TRA ĐA HÌNH =====");

            ql.DisplayAll();


            // =====================================
            // TC05 - GIÁ LĂN BÁNH CAO NHẤT
            // =====================================

            Console.WriteLine("\n===== TC05: GIÁ LĂN BÁNH CAO NHẤT =====");

            PhuongTien max =
                ql.FindMaxGiaLanBanh();

            if (max != null)
            {
                Console.WriteLine(
                    "Phương tiện có giá lăn bánh cao nhất:");

                Console.WriteLine(max.GetInfo());

                Console.WriteLine(
                    $"Giá lăn bánh: {max.TinhGiaLanBanh():N0} VNĐ");
            }


            // =====================================
            // TÌM KIẾM THEO TÊN HÃNG
            // =====================================

            Console.WriteLine("\n===== TÌM KIẾM THEO TÊN HÃNG =====");

            Console.Write("Nhập tên hãng cần tìm: ");

            string keyword =
                Console.ReadLine() ?? "";

            List<PhuongTien> ketQua =
                ql.SearchByName(keyword);

            if (ketQua.Count == 0)
            {
                Console.WriteLine(
                    "Không tìm thấy phương tiện!");
            }
            else
            {
                foreach (PhuongTien pt in ketQua)
                {
                    Console.WriteLine(pt.GetInfo());

                    Console.WriteLine(
                        $"Giá lăn bánh: {pt.TinhGiaLanBanh():N0} VNĐ");

                    Console.WriteLine(
                        "------------------------------------------------");
                }
            }


            Console.WriteLine("\n===== KẾT THÚC CHƯƠNG TRÌNH =====");

            Console.WriteLine(
                "Nhấn phím bất kỳ để thoát...");

            Console.ReadKey();
        }
    }
}
