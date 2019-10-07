# Modul 10 - HTTP Request

Tujuan pembelajaran: Mahasiswa dapat menggunakan beragam class di Java untuk melakukan request HTTP dengan  baik

## Persiapan

- Buat project __Java Application__ baru di NetBeans

## Praktikum

* Kita akan membuat __"robot web"__ yang bisa mengunjungi sebuah halaman web menggunakan __HttpURLConnection__ di Java. Pada praktikum ini kita hanya menggunakan HTTP method __GET__, sedangkan untuk method lain seperti __POST__, __PUT__ dan __DELETE__ dapat Anda pelajari sendiri.
* Buat Class baru dengan mengklik kanan icon __Java Source Package__ di jendela _Project_ lalu pilih __New__ -> __Java Class__. Di bagian Class Name diisi dengan nama __HttpRequests__ dan kemudian tekan tombol __Finish__.
* Ketikkan kode berikut di file __HttpRequests.java__:

```java
package your.app.package;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.UnsupportedEncodingException;
import java.net.HttpURLConnection;
import java.net.URL;
import java.net.URLEncoder;
import java.util.Map;

public class HttpRequests {
    
    private URL url;
    private HttpURLConnection conn;
    private StringBuilder result;
    
    public String httpGet(String url) {
        try {
            this.url = new URL(url);
            prepareConnection("GET");
            readInput();
        } catch (IOException e) { System.out.println(e.getMessage());
        } finally {
            conn.disconnect();
        }
        return result.toString();
    }
    public String httpGet(String url, Map<String, String> query) {
        try {
            this.url = new URL(url + "?" + getParamsString(query));
            prepareConnection("GET");
            readInput();
        } catch (IOException e) { 
            System.out.println(e.getMessage());
        } finally {
            conn.disconnect();
        }
        return result.toString();
    }
    
    private void prepareConnection(String method) throws IOException {
        conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod(method);
        conn.setDoInput(true);
    }
    private void readInput() throws IOException {
        result = new StringBuilder();
        BufferedReader input = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        String line;
        while((line = input.readLine()) != null) {
            result.append(line);
            result.append(System.lineSeparator());
        }
    }
    private String getParamsString(Map<String, String> params) throws UnsupportedEncodingException {
        StringBuilder r = new StringBuilder();
        for (Map.Entry<String, String> entry: params.entrySet()) {
            if (r.length() != 0) {
                r.append("&");
            }
            r.append(URLEncoder.encode(entry.getKey(), "UTF-8"));
            r.append("=");
            r.append(URLEncoder.encode(entry.getValue(), "UTF-8"));
        }
        return r.toString();
    }
    
}
```

* Untuk contoh menggunakan class diatas, ketikkan kode berikut di file __Main.java__ (__class Main__):

```java
package http;

import java.util.HashMap;
import java.util.Map;

public class Main {

    public static void main(String[] args) {
        
        HttpRequests http = new HttpRequests();
        // mengakses situs example.com
        System.out.println(http.httpGet("http://example.com"));
        
        // mengakses api unira untuk mendapatkan data tagihan mhs
        Map<String, String> parameters = new HashMap<>();
        parameters.put("NIM", "2015520004");
        parameters.put("Type", "R");
        System.out.println(http.httpGet("https://api.unira.ac.id/inquiry", parameters));
        
    }
    
}
```

* Jalankan aplikasi dan Anda akan dapat melihat hasil penggunaan beragam Class yang telah kita buat

## Tugas

* Pahami kode modul ini dan modul-modul sebelumnya dan persiapkan ujian praktek dengan baik!