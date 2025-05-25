import java.util.*;
import java.io.*;
import com.google.gson.*; 

abstract class BaseEntity {
    UUID id;
    boolean aktif;

    public BaseEntity() {
        this.id = UUID.randomUUID();
        this.aktif = true;
    }

    public UUID getId() { return id; }
    public boolean isAktif() { return aktif; }
    public void setAktif(boolean aktif) { this.aktif = aktif; }
}

interface DosyaIslemleri {
    void dosyayaKaydetJSON(String dosyaAdi) throws IOException;
    void dosyayaKaydetCSV(String dosyaAdi) throws IOException;
}

// ----- Ucak -----
class Ucak extends BaseEntity implements DosyaIslemleri {
    String model;
    String marka;
    String seriNo;
    int koltukKapasitesi;

    public Ucak(String model, String marka, String seriNo, int koltukKapasitesi) {
        super();
        this.model = model;
        this.marka = marka;
        this.seriNo = seriNo;
        this.koltukKapasitesi = koltukKapasitesi;
    }

    public String getModel() { return model; }
    public String getMarka() { return marka; }
    public String getSeriNo() { return seriNo; }
    public int getKoltukKapasitesi() { return koltukKapasitesi; }

    @Override
    public String toString() {
        return "Ucak: " + marka + " " + model + " SeriNo: " + seriNo + " Kapasite: " + koltukKapasitesi;
    }

    @Override
    public void dosyayaKaydetJSON(String dosyaAdi) throws IOException {
        try (FileWriter fw = new FileWriter(dosyaAdi, true)) {
            Gson gson = new Gson();
            fw.write(gson.toJson(this) + "\n");
        }
    }

    @Override
    public void dosyayaKaydetCSV(String dosyaAdi) throws IOException {
        try (FileWriter fw = new FileWriter(dosyaAdi, true)) {
            fw.write(model + "," + marka + "," + seriNo + "," + koltukKapasitesi + "\n");
        }
    }
}

// ----- Lokasyon -----
class Lokasyon extends BaseEntity implements DosyaIslemleri {
    String ulke;
    String sehir;
    String havaalani;

    public Lokasyon(String ulke, String sehir, String havaalani) {
        super();
        this.ulke = ulke;
        this.sehir = sehir;
        this.havaalani = havaalani;
    }

 
    public String getUlke() { return ulke; }
    public String getSehir() { return sehir; }
    public String getHavaalani() { return havaalani; }

    @Override
    public String toString() {
        return ulke + ", " + sehir + " - " + havaalani;
    }

    @Override
    public void dosyayaKaydetJSON(String dosyaAdi) throws IOException {
        try (FileWriter fw = new FileWriter(dosyaAdi, true)) {
            Gson gson = new Gson();
            fw.write(gson.toJson(this) + "\n");
        }
    }

    @Override
    public void dosyayaKaydetCSV(String dosyaAdi) throws IOException {
        try (FileWriter fw = new FileWriter(dosyaAdi, true)) {
            fw.write(ulke + "," + sehir + "," + havaalani + "\n");
        }
    }
}

// ----- Ucus -----
class Ucus extends BaseEntity implements DosyaIslemleri {
    Lokasyon lokasyon;
    String saat;
    Ucak ucak;
    int mevcutKoltuk;

    public Ucus(Lokasyon lokasyon, String saat, Ucak ucak) {
        super();
        this.lokasyon = lokasyon;
        this.saat = saat;
        this.ucak = ucak;
        this.mevcutKoltuk = ucak.getKoltukKapasitesi();
    }

 
    public Lokasyon getLokasyon() { return lokasyon; }
    public String getSaat() { return saat; }
    public Ucak getUcak() { return ucak; }
    public int getMevcutKoltuk() { return mevcutKoltuk; }

    public boolean rezervasyonYap() {
        if (mevcutKoltuk > 0) {
            mevcutKoltuk--;
            return true;
        }
        return false;
    }

    public void rezervasyonIptalEt() {
        if (mevcutKoltuk < ucak.getKoltukKapasitesi()) {
            mevcutKoltuk++;
        }
    }

    @Override
    public String toString() {
        return lokasyon + " | Saat: " + saat + " | " + ucak + " | Kalan Koltuk: " + mevcutKoltuk;
    }

    @Override
    public void dosyayaKaydetJSON(String dosyaAdi) throws IOException {
        try (FileWriter fw = new FileWriter(dosyaAdi, true)) {
            Gson gson = new Gson();
            fw.write(gson.toJson(this) + "\n");
        }
    }

    @Override
    public void dosyayaKaydetCSV(String dosyaAdi) throws IOException {
        try (FileWriter fw = new FileWriter(dosyaAdi, true)) {
            fw.write(lokasyon.getUlke() + "," + lokasyon.getSehir() + "," + saat + "," + ucak.getModel() + "," + mevcutKoltuk + "\n");
        }
    }
}

// ----- Rezervasyon -----
class Rezervasyon extends BaseEntity implements DosyaIslemleri {
    Ucus ucus;
    String ad, soyad;
    int yas;

    public Rezervasyon(Ucus ucus, String ad, String soyad, int yas) {
        super();
        this.ucus = ucus;
        this.ad = ad;
        this.soyad = soyad;
        this.yas = yas;
    }

   
    public Ucus getUcus() { return ucus; }
    public String getAd() { return ad; }
    public String getSoyad() { return soyad; }
    public int getYas() { return yas; }

    @Override
    public String toString() {
        return "Rezervasyon Sahibi: " + ad + " " + soyad + " | Yaş: " + yas + " | Uçuş: " + ucus;
    }

    @Override
    public void dosyayaKaydetJSON(String dosyaAdi) throws IOException {
        try (FileWriter fw = new FileWriter(dosyaAdi, true)) {
            Gson gson = new Gson();
            fw.write(gson.toJson(this) + "\n");
        }
    }

    @Override
    public void dosyayaKaydetCSV(String dosyaAdi) throws IOException {
        try (FileWriter fw = new FileWriter(dosyaAdi, true)) {
            fw.write(ad + "," + soyad + "," + yas + "," + ucus.getLokasyon().getSehir() + "," + ucus.getSaat() + "\n");
        }
    }
}

// ----- Main -----
public class UcakRezervasyonApp {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // Örnek veriler
        Ucak ucak1 = new Ucak("737", "Boeing", "SN123", 5);
        Ucak ucak2 = new Ucak("A320", "Airbus", "SN456", 4);

        Lokasyon lokasyon1 = new Lokasyon("Türkiye", "İstanbul", "IST");
        Lokasyon lokasyon2 = new Lokasyon("Almanya", "Berlin", "BER");

        Ucus ucus1 = new Ucus(lokasyon1, "10:00", ucak1);
        Ucus ucus2 = new Ucus(lokasyon2, "13:30", ucak2);

        List<Ucus> ucuslar = new ArrayList<>(Arrays.asList(ucus1, ucus2));
        List<Rezervasyon> rezervasyonlar = new ArrayList<>();

        boolean devamEt = true;
        while (devamEt) {
            System.out.println("\n--- Uçak Rezervasyon Sistemi ---");
            System.out.println("1. Mevcut Uçuşları Görüntüle");
            System.out.println("2. Uçuş Rezervasyonu Yap");
            System.out.println("3. Çıkış");
            System.out.print("Seçiminizi yapın: ");

            int anaMenuSecim;
            try {
                anaMenuSecim = scanner.nextInt();
                scanner.nextLine(); // newline karakterini tüket
            } catch (InputMismatchException e) {
                System.out.println("Geçersiz giriş. Lütfen bir sayı girin.");
                scanner.nextLine(); // Hatalı girişi temizle
                continue;
            }

            switch (anaMenuSecim) {
                case 1:
                    mevcutUcuslariGoruntule(ucuslar);
                    break;
                case 2:
                    rezervasyonYap(scanner, ucuslar, rezervasyonlar);
                    break;
                case 3:
                    devamEt = false;
                    System.out.println("Uygulamadan çıkılıyor...");
                    break;
                default:
                    System.out.println("Geçersiz seçenek. Lütfen tekrar deneyin.");
            }
        }

        scanner.close();
    }

    private static void mevcutUcuslariGoruntule(List<Ucus> ucuslar) {
        if (ucuslar.isEmpty()) {
            System.out.println("Henüz kayıtlı uçuş bulunmamaktadır.");
            return;
        }
        System.out.println("\nMevcut Uçuşlar:");
        for (int i = 0; i < ucuslar.size(); i++) {
            System.out.println((i + 1) + ". " + ucuslar.get(i));
        }
    }

    private static void rezervasyonYap(Scanner scanner, List<Ucus> ucuslar, List<Rezervasyon> rezervasyonlar) {
        mevcutUcuslariGoruntule(ucuslar);

        if (ucuslar.isEmpty()) {
            return;
        }

        int secim = -1;
        while (true) {
            System.out.print("Bir uçuş seçin (1-" + ucuslar.size() + "): ");
            try {
                secim = scanner.nextInt();
                scanner.nextLine(); // newline karakterini tüket
                if (secim >= 1 && secim <= ucuslar.size()) {
                    break;
                } else {
                    System.out.println("Geçersiz seçim. Lütfen 1 ile " + ucuslar.size() + " arasında bir sayı girin.");
                }
            } catch (InputMismatchException e) {
                System.out.println("Geçersiz giriş. Lütfen bir sayı girin.");
                scanner.nextLine(); // Hatalı girişi temizle
            }
        }

        Ucus secilenUcus = ucuslar.get(secim - 1);

        if (secilenUcus.rezervasyonYap()) {
            System.out.print("Ad: ");
            String ad = scanner.nextLine();
            System.out.print("Soyad: ");
            String soyad = scanner.nextLine();

            int yas = -1;
            while (true) {
                System.out.print("Yaş: ");
                try {
                    yas = scanner.nextInt();
                    scanner.nextLine(); // newline karakterini tüket
                    if (yas > 0) {
                        break;
                    } else {
                        System.out.println("Geçersiz yaş. Lütfen pozitif bir sayı girin.");
                    }
                } catch (InputMismatchException e) {
                    System.out.println("Geçersiz giriş. Lütfen bir sayı girin.");
                    scanner.nextLine(); // Hatalı girişi temizle
                }
            }

            Rezervasyon rezervasyon = new Rezervasyon(secilenUcus, ad, soyad, yas);
            rezervasyonlar.add(rezervasyon);

            try {
                rezervasyon.dosyayaKaydetJSON("rezervasyonlar.json");
                rezervasyon.dosyayaKaydetCSV("rezervasyonlar.csv");
                System.out.println("Rezervasyon başarıyla oluşturuldu!");
            } catch (IOException e) {
                System.err.println("Dosya yazma hatası: " + e.getMessage());
                secilenUcus.rezervasyonIptalEt();
                rezervasyonlar.remove(rezervasyon);
            }
        } else {
            System.out.println("Üzgünüz, bu uçuşta boş koltuk kalmamıştır.");
        }
    }
}