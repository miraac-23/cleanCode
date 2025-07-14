CleanCode uygulamasında tespit edilen sorunlar;

1-	WeaponType enum kullanımı;
Burada yeni bir silah eklenme durumunda tüm if-else zincirlerine mevcut kodda müdahale gerekiyordu. Burada Open-Closed Prensibi ihlali söz konusuydu

2-	CraftsHuman içinde craftWeapon if-else zinciri;
Silah üretme sorumluluğu insan sınıfında olmamalıdır. Burada hem Single Responsibility hem de Open-Closed prensibi ihlali söz konusudur.

3-	WarriroHuman içinde aynı if-else zinciri;
Silah türüne göre işlem yapmak oop de polimorfizme aykırıdır ve yeni bir silah türü gelme durumunda burası değişecektir.

4-	Kod tekrarları;
if (type == WeaponType.XXX) kodları tekrarlı kodlar olarak uygulamada görülebiliryor.

Uygulamada yapılan değişiklikler;

1-	WeaponType Enum kaldırıldı;
OCP – Open/Closed prensibinde sistemin yeni silahlar için açık, var olan kodun değişmesine kapalı olması gerekir.
Her yeni silah eklendiğinde enuma yeni sabit eklenmesi gerekiyordu. 
Bunun için bir Weapon soyut sınıfı oluşturupher silah için alt sınıflar oluşturuldu ve her silah sınıfının kendi özelliklerini sağlaması sağlandı.
Yeni bir silah eklenmek istenildiğinde yeni bir sınıf tanımlanıp WeaponFactory’e eklemek yeterli olacaktır.

2-	WeaponFactory sınıfı eklendi;

CraftsmanHuman sınıfı içinde silah üretmek, üretim mantığını oraya gömmek demektir. Bu da tek sorumluluk ilkesini ihlal eder. Bu nedenle Factory Design Pattern de göz önünde bulundurularak “WeaponFactory.createWeapon("sword")” gibi çağrılarla merkezi bir üretim sağlandı.

3-	Weapon sınıfı soyut hale getirildi use() methodu her silaha özel olarak tanımlandı;

WarriorHuman sınıfı içinde silah türüne göre özel işlem yapmak polimorfizme aykırıdır.Silahın nasıl kullanılacağına Human değil, silahın kendisi karar vermeli.
LSP – Liskov Substitution prensibine göre her silah Weapon'ın yerini alabilir.

4-	CraftsmanHuman ve WarriorHuman rollerini ayırıldı;

Önceden hem üretici hem kullanıcı Human içinde tanımlanıyordu, bu karmaşa yaratır vet ek sorumluluk ilkesini ihlal eder. Burada değişiklikle beraber CraftHuman sadece üretir, WarriorHuman ise sadece kullanır.

5-	Human sınıfı soyut hale getirildi;

İnsanlar hem üretici hem savaşçı olabilir, ancak farklı davranışları olacak.Bu yüzden ortak özellikleri (weapon, humanName) soyut bir sınıfa alındı. ISP – Interface Segregation prensibi doğrultusunda tüm insan tipleri sadece kendi davranışlarını uygulaması için burası düzenlendi ve son olarak LSP – Liskov Substitution prensibi doğrultusunda her Human alt sınıfı üst sınıf yerine geçebilecek şekilde düzenlendi.

