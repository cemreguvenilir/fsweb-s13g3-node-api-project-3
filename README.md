# Express Ara Yazılı (middleware) Modül Projesi

Bu görevde, bir API oluşturursunuz ve "Minimum Uygulanabilir Ürün" bölümünde listelenen gereksinimleri karşılayan özel ara katman yazılımı yazarsınız.

## Talimatlar

### Görev 1: Proje Kurulumu

Canvas'taki ödev sayfanız, bu projeyi göndermek için talimatlar içeriyor. Bulamadıysanız, Okul Personeli ile iletişime geçin.

### Görev 2: MVP

- `api/server.js` ve `index.js`yi tamamlayarak uygulamayı birbirine bağlayın.
- `api/middleware/middleware.js` içine aşağıda ayrıntıları verilen dört özel ara katman yazılımı işlevi yazın.
- Özel ara yazılımları uygulamadanın uygun yerlerinde kullanın (belirli uç noktalar, tüm yollar veya genel olarak).
- `users-router.js` dosyasında, bir `user` tarafından `posts` listesini almak ve bir `user` için yeni bir `post` depolamak için uç noktalar vardır.

#### Özel Ara Yazılım Gereksinimleri

- `logger()`

  - `logger` her istek(request) hakkında şu bilgileri konsola kaydeder: istek metodu, istek url'si ve bir zaman damgası(timestamp)
  - bu ara yazılım, API'ye yapılan her istekte çalışır

- `validateUserId()`

  - bu ara yazılım, url'de bir `id` parametresi içeren tüm kullanıcı uç noktaları için kullanılacaktır. (örnek: `/api/users/:id` ve bu kimliğe sahip bir kullanıcı olduğundan emin olmak için veritabanını kontrol etmelidir.
  - `id` parametresi geçerliyse, kullanıcı nesnesini `req.user` olarak saklayın ve isteğin devam etmesine izin verin
  - `id` parametresi veritabanındaki herhangi bir kullanıcı kimliğiyle eşleşmiyorsa, `404` durumu ve `{ mesaj: "kullanıcı bulunamadı" }` ile yanıt verin

- `validateUser()`

  - `validateUser`, bir kullanıcı oluşturmak veya güncellemek için bir istekte `body`yi doğrular
  - `istek gövdesinde` gerekli `name` alanı yoksa, `400` durumu ve `{ mesaj: "gerekli name alanı eksik" }` ile yanıt verin

- `validatePost()`

  - `validatePost`, yeni bir gönderi oluşturma isteğinde `body` öğesini doğrular
  - `istek gövdesinde` gerekli `text` alanı yoksa, `400` durumu ve `{ mesaj: "gerekli text alanı eksik" }` ile yanıt verin

### Veritabanı Kalıcılığı Yardımcıları

_users_ ve _posts_ verilerinin kalıcılığını yönetmek için kullanabileceğiniz iki yardımcı dosya vardır. Bu dosyalar `api/users/users-model.js` ve `api/posts/posts-model.js` dosyalarıdır. Her iki dosya da aşağıdaki api'yi yayınlar:

- `get()`: find çağrılması, veritabanında bulunan tüm kaynakların bir dizisine çözümlenen bir Promise verir.
- `getById()`: argüman olarak bir `id` alır ve bulunursa bu kimliğe sahip kaynağa çözümlenen bir Promise verir.
- `insert()`: insert çağırmak, onu bir kaynak nesnesinden geçirmek, onu veritabanına ekler ve yeni kaynağı döndürür.
- `update()`: iki bağımsız değişkeni kabul eder, ilki güncellenecek kaynağın `kimliği` ve ikincisi uygulanacak `değişikliklere` sahip bir nesnedir. Başarılı olduğunda, güncellenmiş kaydı döndürür.
- `remove()`: remove yöntemi, birinci parametresi olarak bir `kimlik` kabul eder ve `kaynağı` veritabanından başarıyla sildikten sonra, silinen kayıt sayısını döndürür.

`users-model.js`, `getUserPosts()` adında, bir kullanıcının `kimliği` iletildiğinde, `user` için tüm `posts` bir listesini döndüren fazladan bir yöntem içerir.

**Tüm yardımcı yöntemler bir Promise verir.**

#### Veritabanı Şeması

`Users` ve `posts` kaynakları için _Veritabanı Şemaları_:

##### Users

| field | data type        | metadata                                              |
| ----- | ---------------- | ----------------------------------------------------- |
| id    | unsigned integer | primary key, auto-increments, generated by database   |
| name  | string           | required, unique                                      |

##### Posts

| field   | data type        | metadata                                            |
| ------- | ---------------- | --------------------------------------------------- |
| id      | unsigned integer | primary key, auto-increments, generated by database |
| text    | text             | required                                            |
| user_id | unsigned integer | required, must be the `id` of an existing user      |

Kaynaklar için test verileri sağladık.

#### Önemli Notlar

- `npm run resetdb` komutunu çalıştırarak veritabanını sıfırlayabilirsiniz.
- Postman veya HTTPie kullanarak çalışmanızı manuel olarak test edin. "npm test" komutuyla otomatik testleri çalıştırın.
- Ek dosyalar oluşturabilirsiniz ancak **mevcut dosyaları veya klasörleri taşımayın veya yeniden adlandırmayın**.
- Ek kitaplıklar yüklemek veya ek komut dosyaları eklemek dışında `package.json` dosyanızı değiştirmeyin. **Mevcut kütüphaneleri güncellemeyin**.
- Uygulamanızda en iyi yöntemleri izlemeniz ve temiz ve profesyonel kodlar yazmanız önemlidir.