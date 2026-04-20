## Commit 1 Reflection Notes
Awalnya `TcpListener` di main menyiapkan port 7878 milik lokal sebagai server untuk menerima koneksi dari browser,
lalu `listener.incoming()` menghasilkan aliran data (`TcpStream`) setiap kali ada klien yang mencoba terhubung ke server tersebut. 

Setalah itu, aliran data (`TcpStream`) akan diproses oleh `handle_connection()` untuk membuat http request dalam bentuk `Vec` (list) dengan cara:  
* stream menntah dibungkus ke dalam buffer dengan `BufReader::new(&mut stream)`  
* stream dipecah jadi baris-baris teks dengan `lines()`  
* teks dari tiap baris di-mapping dan dipastikan tidak error dengan `map(|result| result.unwrap())`
* `take_while(|line| !line.is_empty())` membuat mapping berhenti jika sudah menemukan baris kosong. dengan kata lain, kita hanya mau ambil Header HTTP nya saja, tidak ambil Body HTTP.
* semua baris yang sudah di-mapping akan dikumpulkan ke `Vec` yang akan menjadi http request dengan `collect()` 