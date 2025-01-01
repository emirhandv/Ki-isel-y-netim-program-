# kisisel yonetim programi
#nesneye yönelik proglama proje ödevi
using System;
using System.Collections.Generic;

namespace KisiselFinansYoneticisi
{
    // Gelir veya gider kaydını temsil eden sınıf
    public class Islem
    {
        public DateTime Tarih { get; private set; }
        public string Aciklama { get; private set; }
        public decimal Miktar { get; private set; }

        public Islem(string aciklama, decimal miktar)
        {
            Tarih = DateTime.Now;
            Aciklama = aciklama;
            Miktar = miktar;
        }

        public override string ToString()
        {
            return $"[{Tarih:yyyy-MM-dd HH:mm}] {Aciklama}: {Miktar:C}";
        }
    }

    // Gelir ve gider yönetimi yapan sınıf
    public class ButceYoneticisi
    {
        private readonly List<Islem> _islemler;

        public ButceYoneticisi()
        {
            _islemler = new List<Islem>();
        }

        public void GelirEkle(string aciklama, decimal miktar)
        {
            if (miktar <= 0)
                throw new ArgumentException("Gelir miktarı pozitif olmalıdır.");

            _islemler.Add(new Islem(aciklama, miktar));
            Console.WriteLine("Gelir başarıyla eklendi!");
        }

        public void GiderEkle(string aciklama, decimal miktar)
        {
            if (miktar <= 0)
                throw new ArgumentException("Gider miktarı pozitif olmalıdır.");

            _islemler.Add(new Islem(aciklama, -miktar));
            Console.WriteLine("Gider başarıyla eklendi!");
        }

        public void OzetGoster()
        {
            decimal toplamGelir = 0;
            decimal toplamGider = 0;

            Console.WriteLine("\n--- Tüm İşlemler ---");
            foreach (var islem in _islemler)
            {
                Console.WriteLine(islem);
                if (islem.Miktar > 0)
                    toplamGelir += islem.Miktar;
                else
                    toplamGider += islem.Miktar;
            }

            Console.WriteLine("\n--- Özet ---");
            Console.WriteLine($"Toplam Gelir: {toplamGelir:C}");
            Console.WriteLine($"Toplam Gider: {Math.Abs(toplamGider):C}");
            Console.WriteLine($"Net Bakiye: {toplamGelir + toplamGider:C}");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            var yonetici = new ButceYoneticisi();

            while (true)
            {
                Console.WriteLine("\n--- Kişisel Finans Yönetimi ---");
                Console.WriteLine("1. Gelir Ekle");
                Console.WriteLine("2. Gider Ekle");
                Console.WriteLine("3. Özet Görüntüle");
                Console.WriteLine("4. Çıkış");
                Console.Write("Seçiminiz: ");

                string secim = Console.ReadLine();

                try
                {
                    switch (secim)
                    {
                        case "1":
                            Console.Write("Gelir Açıklaması: ");
                            string gelirAciklama = Console.ReadLine();
                            Console.Write("Gelir Miktarı: ");
                            decimal gelirMiktar = decimal.Parse(Console.ReadLine());
                            yonetici.GelirEkle(gelirAciklama, gelirMiktar);
                            break;

                        case "2":
                            Console.Write("Gider Açıklaması: ");
                            string giderAciklama = Console.ReadLine();
                            Console.Write("Gider Miktarı: ");
                            decimal giderMiktar = decimal.Parse(Console.ReadLine());
                            yonetici.GiderEkle(giderAciklama, giderMiktar);
                            break;

                        case "3":
                            yonetici.OzetGoster();
                            break;

                        case "4":
                            Console.WriteLine("Çıkış yapılıyor...");
                            return;

                        default:
                            Console.WriteLine("Geçersiz seçim. Lütfen tekrar deneyin.");
                            break;
                    }
                }
                catch (Exception ex)
                {
                    Console.WriteLine($"Hata: {ex.Message}");
                }
            }
        }
    }
}

