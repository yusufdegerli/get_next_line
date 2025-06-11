# get_next_line

## Proje Hakkında
**get_next_line**, 42 Network müfredatında yer alan bir C programlama projesidir. Bu proje, bir dosya tanımlayıcısından (file descriptor) satır satır okuma yapan bir `get_next_line` fonksiyonu geliştirmeyi amaçlar. Proje, dosya okuma, dinamik bellek yönetimi ve statik değişken kullanımı gibi C programlamanın temel kavramlarını öğrenmek için tasarlanmıştır. Kendi `get_next_line` fonksiyonumu yazarak, dosya işleme, hafıza yönetimi ve algoritmik düşünme becerilerimi geliştirdim.

## Özellikler
`get_next_line` fonksiyonu, aşağıdaki işlevselliği sağlar:

### Zorunlu Kısım
- **Fonksiyon Prototipi**:
  ```c
  char *get_next_line(int fd);
  ```
- **İşlevsellik**:
  - Bir dosya tanımlayıcısından (file descriptor, `fd`) bir satırı okur ve döndürür.
  - Satır, yeni satır karakteri (`\n`) ile biterse, bu karakter dahil edilir.
  - Dosya sonuna (EOF) ulaşıldığında veya hata oluştuğunda `NULL` döndürür.
  - Birden fazla dosya tanımlayıcısını aynı anda destekler (örneğin, farklı dosyalar için ayrı ayrı okuma).
- **Kısıtlamalar**:
  - Buffer boyutu (`BUFFER_SIZE`), derleme zamanında tanımlanır.
  - Standart C kütüphanesinin yalnızca `read`, `malloc` ve `free` fonksiyonları kullanılabilir.
  - Statik değişken kullanımıyla, bir sonraki satır için kalan veriler saklanır.

### Bonus Kısım (Opsiyonel)
- Tek bir statik değişken kullanarak birden fazla dosya tanımlayıcısını yönetme.
- `BUFFER_SIZE` değerinden bağımsız olarak doğru çalışabilme.
- Hata durumlarında (örneğin, geçersiz `fd` veya bellek tahsis hatası) güvenilir davranış.

**Not**: Bonus kısmı, projenizin kapsamına bağlı olarak eklendi veya eklenmedi. Eğer bonuslar implemente edildiyse, lütfen belirtin, README'yi güncelleyebilirim.

## Kurulum ve Kullanım

### Gereksinimler
- C derleyicisi (gcc veya clang önerilir).
- POSIX uyumlu bir işletim sistemi (Linux, macOS veya WSL).
- `make` komutu (isteğe bağlı, test için).

### Kurulum
1. Repoyu klonlayın:
   ```bash
   git clone https://github.com/yusufdegerli/get_next_line.git
   cd get_next_line
   ```
2. Proje, bağımsız bir fonksiyon olarak kullanılabilir. Test etmek için bir `main.c` dosyası oluşturun:
   ```c
   #include "get_next_line.h"
   #include <fcntl.h>
   #include <stdio.h>

   int main(void)
   {
       int fd = open("test.txt", O_RDONLY);
       char *line;
       while ((line = get_next_line(fd)))
       {
           printf("%s", line);
           free(line);
       }
       close(fd);
       return (0);
   }
   ```
3. Derleyin ve çalıştırın:
   ```bash
   gcc main.c get_next_line.c get_next_line_utils.c -o gnl
   ./gnl
   ```
   - Buffer boyutunu tanımlamak için:
     ```bash
     gcc -D BUFFER_SIZE=42 main.c get_next_line.c get_next_line_utils.c -o gnl
     ```

### Test Dosyası Oluşturma
Test için bir `test.txt` dosyası oluşturabilirsiniz:
```bash
echo -e "Birinci satır\nİkinci satır\nÜçüncü satır" > test.txt
```

## Test Etme
Projenizi test etmek için aşağıdaki açık kaynak test araçlarını kullanabilirsiniz:
- [Tripouille/gnlTester](https://github.com/Tripouille/gnlTester): Kapsamlı birim testleri ve edge case'ler.
- [Mazoise/42TESTERS-GNL](https://github.com/Mazoise/42TESTERS-GNL): Hata durumları ve performans testleri.
- Kendi test dosyanızı yazabilirsiniz (örneğin, farklı `BUFFER_SIZE` değerleri veya birden fazla dosya tanımlayıcısı ile).

Test araçlarını kullanmak için:
1. Test reposunu klonlayın.
2. Test talimatlarını takip ederek `get_next_line` fonksiyonunuzu entegre edin.
3. Testleri çalıştırın ve çıktıları kontrol edin.

## Kod Yapısı
Proje, modüler bir yaklaşımla yazılmıştır:
- **get_next_line.c**: Ana fonksiyon ve satır okuma mantığı.
- **get_next_line_utils.c**: Yardımcı fonksiyonlar (örneğin, string birleştirme, yeni satır kontrolü).
- **get_next_line.h**: Fonksiyon prototipleri ve gerekli tanımlar.

### Örnek Kullanım
```c
#include "get_next_line.h"
#include <fcntl.h>
#include <stdio.h>

int main(void)
{
    int fd = open("test.txt", O_RDONLY);
    char *line;

    line = get_next_line(fd);
    printf("1. Satır: %s", line);
    free(line);

    line = get_next_line(fd);
    printf("2. Satır: %s", line);
    free(line);

    close(fd);
    return (0);
}
```

**Çıktı** (test.txt içeriğine bağlı olarak):
```
1. Satır: Birinci satır
2. Satır: İkinci satır
```

## Öğrenilenler
Bu proje sayesinde aşağıdaki becerileri geliştirdim:
- **C Programlama**: Dosya okuma (`read`), statik değişkenler ve dinamik bellek yönetimi.
- **Hafıza Yönetimi**: Bellek tahsisi, serbest bırakma ve sızıntı önleme.
- **Modüler Kod**: Okunabilir, yeniden kullanılabilir ve bakımı kolay kod yazımı.
- **Hata Ayıklama**: Edge case'ler (örneğin, boş dosya, büyük dosyalar, geçersiz `fd`) için test ve düzeltme.
- **Algoritmik Düşünme**: Kalan verileri saklama ve satır ayrıştırma mantığı.

## Katkıda Bulunma
Eğer bu projeye katkıda bulunmak isterseniz:
1. Repoyu fork edin.
2. Yeni bir dal (branch) oluşturun: `git checkout -b feature/yeni-ozellik`.
3. Değişikliklerinizi yapın ve commit edin: `git commit -m "Yeni özellik eklendi"`.
4. Dalınızı push edin: `git push origin feature/yeni-ozellik`.
5. Bir Pull Request açın.

## Lisans
Bu proje, [MIT Lisansı](LICENSE) altında lisanslanmıştır.

**Not**: Bu README, 42 Network'ün get_next_line projesi için hazırlanmıştır. Projenizi 42 kurallarına uygun şekilde teslim ediyorsanız, lütfen 42'nin GitHub paylaşım politikalarını kontrol edin.
