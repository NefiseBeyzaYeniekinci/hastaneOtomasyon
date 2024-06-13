# hastaneOtomasyon
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Calendar;
import java.util.List;

// Poliklinik sınıfı
class Poliklinik {
    private String adi;

    public Poliklinik(String adi) {
        this.adi = adi;
    }

    public String getAdi() {
        return adi;
    }
}

// Ana Hastane Sınıfı
public class HastaneOtomasyonu {
    private static List<Poliklinik> poliklinikler = new ArrayList<>();
    private static JFrame frame;
    private static JTextField textFieldKullaniciAdi;
    private static JComboBox<String> comboBoxSaat;
    private static JSpinner spinnerTarih;

    public static void main(String[] args) {
        // Örnek poliklinikler
        String[] poliklinikAdlari = {
                "Beslenme ve Diyet", "Beyin ve Sinir Cerrahisi", "Çocuk Sağlığı ve Hastalıkları", "Dermatoloji (Cildiye)",
                "Endokrinoloji ve Metabolizma", "Enfeksiyon Hastalıkları", "Fiziksel Tıp ve Rehabilitasyon",
                "Gastroenteroloji", "Genel Cerrahi", "Geriyatri",
                "Göğüs Cerrahisi", "Göğüs Hastalıkları", "Göz Hastalıkları", "Hematoloji", "İç Hastalıkları",
                "Kadın Hastalıkları ve Doğum", "Kalp ve Damar Cerrahisi", "Kardiyoloji",
                "Kulak-Burun-Boğaz Hastalıkları", "Nefroloji", "Nöroloji", "Ortopedi ve Travmatoloji",
                "Plastik Rekonstrüktif ve Estetik Cerrahi", "Psikiyatri", "Radyasyon Onkolojisi", "Tıbbi Genetik","Ağız ve Diş Sağlığı",
                "Tıbbi Patoloji", "Üroloji"
        };

        // Poliklinikler listesi
        for (String poliklinikAdi : poliklinikAdlari) {
            poliklinikler.add(new Poliklinik(poliklinikAdi));
        }

        // Swing arayüzü
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                createAndShowGUI();
            }
        });
    }

    private static void createAndShowGUI() {
        frame = new JFrame("Hastane Otomasyonu");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        // Panel 
        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(5, 2, 10, 10)); // 5 satır, 2 sütun

        // Kullanıcı adı girişi için JTextField 
        JLabel labelKullaniciAdi = new JLabel("Kullanıcı Adı:");
        textFieldKullaniciAdi = new JTextField(20);

        // Saat seçimi için JComboBox 
        JLabel labelSaat = new JLabel("Randevu Saati:");
        String[] saatler = {"08:00", "08:30", "09:00", "09:30", "10:00", "10:30", "11:00", "11:30", "12:00", "12:30",
                            "13:00", "13:30", "14:00", "14:30", "15:00", "15:30", "16:00"};
        comboBoxSaat = new JComboBox<>(saatler);

        // Tarih seçimi için JSpinner 
        JLabel labelTarih = new JLabel("Randevu Tarihi:");
        SpinnerDateModel model = new SpinnerDateModel();
        spinnerTarih = new JSpinner(model);
        JSpinner.DateEditor editor = new JSpinner.DateEditor(spinnerTarih, "dd/MM/yyyy");
        spinnerTarih.setEditor(editor);

        // Poliklinik seçimi için JComboBox 
        JLabel labelPoliklinik = new JLabel("Poliklinik Seçiniz:");
        JComboBox<String> comboBoxPoliklinik = new JComboBox<>();
        for (Poliklinik poliklinik : poliklinikler) {
            comboBoxPoliklinik.addItem(poliklinik.getAdi());
        }

        // Randevu al butonu
        JButton randevuAlButton = new JButton("Randevu Al");
        randevuAlButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String kullaniciAdi = textFieldKullaniciAdi.getText().trim();
                String secilenSaat = (String) comboBoxSaat.getSelectedItem();
                Calendar selectedDate = Calendar.getInstance();
                selectedDate.setTime((java.util.Date) spinnerTarih.getValue());
                SimpleDateFormat dateFormat = new SimpleDateFormat("dd/MM/yyyy");
                String tarih = dateFormat.format(selectedDate.getTime());
                String secilenPoliklinik = (String) comboBoxPoliklinik.getSelectedItem();
                randevuAl(kullaniciAdi, secilenPoliklinik, tarih, secilenSaat);
            }
        });

        // Bileşenleri panele ekleme
        panel.add(labelKullaniciAdi);
        panel.add(textFieldKullaniciAdi);
        panel.add(labelSaat);
        panel.add(comboBoxSaat);
        panel.add(labelTarih);
        panel.add(spinnerTarih);
        panel.add(labelPoliklinik);
        panel.add(comboBoxPoliklinik);
        panel.add(randevuAlButton);

        // Paneli frame'e ekleme
        frame.add(panel, BorderLayout.CENTER);

        // Frame'i görünür yapma
        frame.pack(); // Bileşenlerin boyutlarına göre frame'in boyutunu ayarlar
        frame.setVisible(true);
    }

    private static void randevuAl(String kullaniciAdi, String secilenPoliklinik, String tarih, String secilenSaat) {
        JOptionPane.showMessageDialog(frame, "Sayın " + kullaniciAdi + ", " + tarih + " tarihinde saat " +
                secilenSaat + " için " + secilenPoliklinik + " Polikliniğine randevunuz alınmıştır.\nGeçmiş Olsun...");
    }
}
