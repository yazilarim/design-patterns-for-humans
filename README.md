![Design Patterns For Humans](https://cloud.githubusercontent.com/assets/11269635/23065273/1b7e5938-f515-11e6-8dd3-d0d58de6bb9a.png)

***
<p align="center">
🎉 Çeviri henüz tamamlanmamıştır.! 🎉
</p>
<p align="center">
🎉 En basit şekilde tasarım kalıpları! 🎉
</p>
<p align="center">
Tasarım kalıpları kompleks güzel bir yazilim başlığı konusudur. <i>En basit</i> şekilde tasarım kalıplarını anlatmaya çalışacağım.
</p>

***

Giriş
=================

Tasarım kalıpları *sürekli tekrarlanan* konular çözüm yöntemleri sunar. Tasarım kalıpları belirli problemlere karşlılık belirli çözümler sunan kalıplardır.

Tasarım kalıpları proje ekleyebileceğiniz belli bir class, namespace veya dll değildir!!

> Tasarım kalıpları sürekli tekrarlan problem için çözüm yöntemi sunar bununla birlikte her uygulandığı problem farklılaşabilir. Yani problem genel, uygulanacağı içerik değişebilir.


> Tasarım kalıpları belli bir problemi çözmek için kabataslak sunar, ve kabataslağın uygulandığı durumlar her zaman birbirinden değişiktir.

⚠️ Aman Dikkat
-----------------
- Tasarım kalıpları her problemin çözümü değildir.
- Tasarım kalıplarını uygulamak için fırsat aramayın, Probleminizi tasarım kalıplarına göre düşünmeyin. 
- Tasarım kalıpları problemler için çözüm yolu sağlar, tasarım kalıplarını uygulayacağım diye problem yaratmayın.
- Tasarım kalıpları doğru *durumda* doğru probleme uygulanırsa mükemmel çözümlerdir, ama eğer yanlış durum ya da yanlış probleme uygulanırsa kötü kod kokusun kaynağı olabilirler! 

> Aşağıda kodlar c# ile yazılmıştır ama asla kendi yazılım kabilenizin diline uygulamaktan kaçınmayın.

Tasarım kalıpları tipleri
-----------------

* [Yaratımsal Tasarım Kalıpları](#Yaratımsal-Tasarım-Kalıpları)
* [Structural](#structural-design-patterns)
* [Behavioral](#behavioral-design-patterns)

#Yaratımsal-Tasarım-Kalıpları
==========================

Yazıya dökersek :
> Yaratımsal kalıplar bir objeninin nasıl örnekleneceği problemine veya bir grup nesnenin nasıl örneklenebileceği problemine çözüm sunar .


 * [Basit-Fabrika (Simple Factory)](#-basit-fabrikasimple-factory)
 * [Fabrika Metodu (Factory Method)](#-fabrika-metodu-factory-method)
 * [Soyut Fabrika (Abstract Factory)](#-soyut-fabrika-abstract-factory)
 * [İnşa Edici (Builder)](#-i̇nşa-edici-builder)
 * [Prototip (Prototype)](#-prototip-prototype)
 * [Tekil (Singleton)](#-tekil-singleton)

🏠 #Basit-Fabrika(Simple Factory)
--------------
Gerçek dünya senaryosu
> Varsayılımki, bir ev inşa ediyorsunuz ve ev inşa etmek için kapılara ihtiyacınız var. Bunun için ihtiyacınız olan şeyler ise : biraz tahta, yapıştırıcı ve çivi (ve kapı yapmak için gereken diğer araçlar). Kapıyı gerekli eşyaları kullanarak daha sonra yapabilirsiniz **ama** kapıyı kendiniz yapmak yerine bir basit bir fabrikadan yani bir marangozhaneden isteyebilirsiniz. Marangozhaneden istediğiniz zaman eviniz içerisindeki yapacağınız bir sürü kir ve işten kutulmuş olursunuz. 


Basitçe : 
>Basit Fabrika  istediğiniz nesnenin nasıl yaratılacağı mantığından ve yapacağınız efordan sizi kurtarır.
>Sizi yani istemiciyi(veya client'ı) nesnenin nasıl yaratılacağı karşmasından kurtarır.
>Nesne tabanlı programlamada(bundan sonra OOP diyeceğim), bir nesne olan *fabrika* diğer objeleri ve obje grubunu yaratmak için kullanılır.
>Kullanacağımız dil olan c# ile nesne yaratma işlemini new keyword ile yapılmaktadır ama kendi dilinizi kullanarak da bu yapıyı uygulayabilirsiniz.


**Yazılım Örneği**

Öncelilikle bizim kapı arayüzü(interface) ve uygulaması(implemantation'ı) var.
```csharp
public interface Door
{

}

public class WoodenDoor : Door
{
    private float _width { get;  }
    private float _height { get; set; }
    public WoodenDoor(float width,float height)
    {
        _width = width;
        _height = height;
    }
    public KeyValuePair<float, float> GetWidthAndHeight()
    {
        return new KeyValuePair<float, float>(_width,_height);
    }
}
```

Daha sonra kapı fabrikamızı(örneğimizdeki marangozhane oluyor bu class) yaratalım : 

```csharp
public class DoorFactory
{
    public static Door MakeDoor(float width,float height)
    {
        return new WoodenDoor(width, height);
    }
}
```
Daha sonra aşağıdaki gibi basit fabrikamızı kullanabiliriz: 

```csharp
// 75f(genislik)x200(yuksekliğinde) bir kapı yarattık.
var door = DoorFactory.MakeDoor(75f,200f);
```

**Ne zaman kullanılmalı?**

Obje yaratırken bir çok parametremiz olduğunda, ve iş mantığı(bussiness logic) karmaşıklaştığı zaman; nesne yaratma işlemini bir fabrikaya koymak, kod tekrarını önlemek için çok yararlı olacaktır.


🏭 #Fabrika-Metodu (Factory Method)
--------------
Gerçek dünyası senaryosu 1 : 

> Bir genel müdürün Developer işe alacağını düşünelim. Genel müdür developer işe alacak yani programlama açısından yaratacaktır. Porgramatik açıdan ile genel müdür, developer yaratacak olan fabrika metodunu sahibi olacaktır. 

Kod'un mantığı :
> Genel Müdür factory method'u üzerinde tutar.
> İşe alım yapacak(developer yaratacak) olan bu *kişi*(Genel Müdür) fabrika metoduna sahip olacak olan nesnedir. İşe alım yapacak olan kişi "Genel Müdür"dür. 
> Bu yüzden "Genel Müdür" fabrika metodunu kendi üstünde tutar.

**Yazılım Örneği**
İlk başta çalışan yapımızı kuralım : 

```csharp
public interface Calisan
{
    void DoSomeWork();
}

public class Developer : Calisan
{
    public void DoSomeWork()
    {
        System.Console.WriteLine("Kodlar yazılıyor");
    }
}
```

Factory Method'u kodlayalım : 

```csharp

public abstract class HireManager
{
    //Factory Method
    public abstract Calisan IseAlimYap();
}

public class SirketMuduru : HireManager
{
	//Factory Method
    public Calisan IseAlimYap()
    {
    	return new Developer();
    }
}
```

Client Kodu : 

```csharp
var mudur = new SirketMuduru();
var developer = mudur.IseAlimYap();
```

**Ne zaman kullanılı?**
Client'ın nesne yaratma sorumluluğunu başka bir nesneye akatarmasını istediği zaman kullanır.
____________________


Gerçek dünya senaryosu 2 : 

> Bir genel müdürün Developer ve/veya Analist işe alacağını düşünelim.   Genel Müdür işe alacağı kişilerin pozisyonuna bağlı olarak kişiyi seçmesi gerekir. 
> İşe alım yapacak(işçi yaratacak) olan bu kişi fabrika metoduna sahip olacak olan nesnedir.

Kod'un mantığı
> - Factory Method'da hangi nesnenin yarataılacağına karar veren  "Genel Mudur"dür çünkü işe alımdan sorumlu tutacağı kişinin bilgisi ve **işe alınacak pozisyonun** bilgisi tutar.
> - GenelMüdürün bilmesi gereken tek şey işe alım yapacağı pozisyon ve işe alım yapacak kişinin bilgisidir. Bu bilgilere göre işçi alacaktır(yaratacaktır).
> Pozison bazında işe alınacak kişinin seçildiği(yaratıldığı) metoda FactoryMetodu denir.
> 
> Örneğimizde işe alma sorumluluğu olan, soyut sınıf olan ,HireManager'ın abstract olarak işaretlenen factory metodunun, alt sınıflarda(derived/child sınıflarda) spesifik bir pozisyondan işe alınacak olan çalışan örneklenmesini sağlayan metoda fabrika metodur denir.
> 
> Factory method HireManager'ların üzerindedir. Factory metot Calisan yaratır(create eder). Genel Müdür aynı zamanda bir HireManager'dır. 


**Yazılım Örneği**
İlk başta çalışan yapımızı kuralım : 

```csharp
public interface Calisan
{
    void DoSomeWork();
}

public class Developer : Calisan
{
    public void DoSomeWork()
    {
        System.Console.WriteLine("Kodlar yazılıyor");
    }
}

public class Analist : Calisan
{
    public void DoSomeWork()
    {
        System.Console.WriteLine("Analiz yapılıyor.");
    }
}
```

Factory Method'ı kodlayalım : 

```csharp

public abstract class HireManager
{
    //Factory Method
    public abstract Calisan IseAlimYap(CalisanTipi calisanTipi);
}

public class SirketMuduru : HireManager
{

	//Factory Method
    public Calisan IseAlimYap(CalisanTipi calisanTipi)
    {
        Calisan calisan = null;
        if (calisanTipi == CalisanTipi.Developer)
        {
            calisan = new Developer();
        }
        if (calisanTipi == CalisanTipi.Analist)
        {
            calisan = new Analist();
        }
        return calisan;
    }
}
public enum CalisanTipi
{
    Developer,
    Analist
}
```


Client Kodu : 

```csharp
var mudur = new SirketMuduru();
mudur.IseAlimYap(CalisanTipi.Analist);
```

**Ne zaman kullanılmalı ?**
Eğer calisma zamanında (runtime'da) client hangi sınıfı yaratmak isteyeceğini bilmiyorsa fabrika metotları yararlı olabilir.
Aynı zamanda nesne yaratmayı client'tan alıp factory method'a yüklendiğinden, client kodunda sadeleşmede de sağlar, temiz kod (clean code) yazılmasına yardımcı olur.



🔨 #Soyut-Fabrika (Abstract Factory)
----------------


Gerçek dünya seneryosu: 
> Bir Genel Müdürün Developer ve/veya Analist işe alacağını düşünelim.  Genel Müdürün Developer ikiye ayrı grup olarak işe alım yapmak istiyor. Developer Grubu Front End ve Back End olarak ikiye ayrılmıştır.  Genel Müdür işe alacağı kişilerin pozisyonuna bağlı olarak kişiyi işe alması gerekmektedir. 
> Artık Genel Müdür işçi alma(yaratma) işlemini başka bir kişiye devretmek istiyor.
> Bu durumda Genel Müdür artık client olup işe alma sürecini HireManager'lara bırakmaksı gerekir.
> İşe alım yapacak(işçi yaratacak) olan bu kişi fabrika metoduna sahip olacak olan nesnedir.

Basitçe : 
> Abstract Factory : Fabrika metotlardan oluşan bir class'ıdır. 
> 
> Abstract Factory : Birbirleriye yakın ilşikisi olan nesneleri yaratım problemini bir araya toplar, birbirleriyle ilişkisi olmayan yapılar aynı abstract factory'ye koyulmamalıdır!!!
> 
> Abstract Factory : tekil(yani örneğimizdeki Analist) veya birliktelik ifade eden (Front End developer ve Back End developer) nesneleri yaratılma işlemini alt-class'lara yükler.

**Yazılım Örneğin?**

Çalışan yapımız aşağıdaki şekildedir : 
```csharp
public interface Calisan
{
    void DoSomeWork();
}

public class FrontEndDeveloper : Calisan
{
    public void DoSomeWork()
    {
        System.Console.WriteLine("Ui yazılıyor");
    }
}

public class BackEndDeveloper : Calisan
{
    public void DoSomeWork()
    {
        System.Console.WriteLine("API yazılıyor");
    }
}

public class Analist : Calisan
{
    public void DoSomeWork()
    {
        System.Console.WriteLine("Analiz yapılıyor.");
    }
}
```

Calisan runtime da gelen parametreye göre örneklenebilmesi için bir enum oluşturdum, eğer örneklemek istediğini yapıyı runtime ne olacağını bilemiyorsanız bir enum yaratıp yaratılacak nesnenin tipinin enum'a devredebilirsiniz.
```csharp
public enum CalisanTipi
{
    FrontEndDeveloper,
    BackEndDeveloper,
    Analist
}
```

Şimdi ise developer oluşturacaK  soyut fabrikayı (abstract factory) yaratalım : 
```csharp

public abstract class AbstractDeveloperHireManager
{
    //Factory Method1
    public abstract Calisan HireFrontEndDeveloper();
    //Factory Method2
    public abstract Calisan HireBackEndDeveloper();
}

public class ConcreteDeveloperHireManager : AbstractDeveloperHireManager
{
    protected int DogruDeveloperCevap { get; } = 2;
    // Factory Method1
    public override Calisan HireFrontEndDeveloper()
    {
        var iseAlindiMi = SoruSorFrontEndDeveloper();
        if (iseAlindiMi)
            return new FrontEndDeveloper();
        return null;
    }
    // Factory Method2
    public override Calisan HireBackEndDeveloper()
    {
        var iseAlindiMi = SoruSorBackEndDeveloper();
        if (iseAlindiMi)
            return new BackEndDeveloper();
        return null;
    }
    #region Yardımcı metotlar
    protected bool SoruSorFrontEndDeveloper()
    {
        System.Console.WriteLine("1+1 = ?");
        var cevap = System.Console.Read();
        if(cevap==DogruDeveloperCevap)
            return  true;
        return false;
    }
    protected bool SoruSorBackEndDeveloper()
    {
        System.Console.WriteLine("0+2 = ?");
        var cevap = System.Console.Read();
        if(cevap==DogruDeveloperCevap)
            return  true;
        return false;
    }
    #endregion

}
```

Daha sonra analist yaratan soyut fabrikayı yaratalım : 

```csharp

public abstract class AbstractAnalistHireManager
{
    //Factory Method
    public abstract Calisan HireAnalist();
}

public class ConcreteAnalistHireManager : AbstractAnalistHireManager
{
    public string DogruCevap { get;  } = "Problemi tespit etmek.";
    public override Calisan HireAnalist()
    {
        if (SoruSor("Analist il görevi nedir?"))
        {
            return new Analist();
        }
        return null;
    }

    private bool SoruSor(string soru)
    {
        System.Console.WriteLine(soru);
        var cevap = Console.ReadLine();
        if (cevap == DogruCevap)
        {
            return true;
        }
        return false;
    }
}
```

Client kodu :

```csharp
public class SirketMuduru
{
    public Calisan Hire(CalisanTipi calisanTipi)
    {
        var analist = calisanTipi == CalisanTipi.Analist;
        var frontEnd = calisanTipi == CalisanTipi.FrontEndDeveloper;
        var backEnd = calisanTipi == CalisanTipi.FrontEndDeveloper;
        if (frontEnd || backEnd)
        {
            AbstractDeveloperHireManager developerFactory = new  ConcreteDeveloperHireManager();
            if (frontEnd)
            {
                var frontEndDeveloper = developerFactory.HireFrontEndDeveloper();
                return frontEndDeveloper;
            }
            if (backEnd)
            {
                var backEndDeveloper = developerFactory.HireBackEndDeveloper();
                return backEndDeveloper;
            }
        }
        if (analist)
        {
            AbstractAnalistHireManager analistHireManager = new ConcreteAnalistHireManager();
            var analizci = analistHireManager.HireAnalist();
            return analizci;
        }
        return null;
    }
}
```

Abstract factory birbirlye ilşkili olan nesnelerin yaratımını kapsüller,böylece hem karmaşa önlenir hem de daha temiz bir kod yapısına geçilebilir.

**Ne zaman kullanılmalı ?**
İlişkili nesnelerin veya bir nesne ailesinin olduğu yerde abstract factory kullanılabilir. Örneğimiz nesne ailesi developer'lardır.

👷 #İnşa-Edici (Builder)
--------------------------------------------
Açıklama :
> Builder pattern'inde aslında bir inşa sürecinden bahsedilmektedir. Yani bir nesne yaratırken o nesnenin bir yaratım **süreci** vardır. Builder bir süreç içerisinde nesneye inşa eder(build eder).


Gerçek Dünya Senaryosu:
> Bir kahve yapacağımızı düşünlelim ihtiacımız olan kahve, su veya süttür.  Ama biz bu üçünün hepsiye ve sadece ikisiyle de kahve yapabiliriz. Ama bizim kahveyi yapmamız bir süreçtir ilk başta su ve/veya süt ısıtılacak ardından bardağa dökülecek ve su ve/veya süt üzerine kahve atılacak ve karıştırlıması gerekecektir. Bu sebeple kahve yapmak bir **süreç** gerektiren bir iştir.

Basitçe:
> İnşa eden (Builder) tasarım kalıbı bir süreç ile inşa edilecek olan nesnenin *yaratılma sürecine* vurgu yapmaktadır. 

Aynı zamanda : 
> (Telescoping constructor anti-pattern) Teleskop constructor anti pattern'inin üstesinden de gelmektedir. Çünkü az sonra göreceğimiz üzere nesne yaratım süreçi metotlar ile yapılmaktadır, bu sebep ile constructor yerine anlamı metot isimleri kullanarak daha açıklayıcı kod yazabiliriz.

Yazılım Örneği : 

Öncelikle bardak kahve class'ını oluşturalım :
```csharp
class BardakKahve
{
    public float SuMiktari { get; set; }
    public decimal KahveGramaj { get; set; }
    public float SütMiktari { get; set; }
}
```

Şimdi ise builder yaratalım : 
```csharp
class KahveMakinesi
{
    private BardakKahve _kahve;

    public KahveMakinesi()
    {
        _kahve= new BardakKahve();
    }

    public KahveMakinesi SuKoy(float SuMiktari)
    {
        _kahve.SuMiktari= SuMiktari;
        return this;
    }

    public KahveMakinesi KahveKoy(decimal kahveMiktari)
    {
        _kahve.KahveGramaj= kahveMiktari;
        return this;
    }
    
    public KahveMakinesi SütKoy(float SütMiktari)
    {
        _kahve.SütMiktari= SütMiktari;
        return this;
    }

    public BardakKahve Build()
    {
        return _kahve;
    }
}

```

Kullanımı : 
```csharp
	KahveMakinesi makina = new KahveMakinesi();
	var latte =  makina.SuKoy(90f).KahveKoy(10m).SütKoy(90f).Build();
```

**Ne zaman kullanılmalı ?**
Nesne yaratmanın bir süreç olduğu her yerde kullanılabilir. Aynı zamanda nesne yaratırken metot isimleri kullanıldığından nesneyi yaratma sürecini açık hala getirir(dışarıya expose eder).

🤖 #prototip (Prototype)
------------
Gerçek dünya senaryosu : 
> Yıl 3542 insanlar sonunda doğayla uyum içinde yaşamayı başarmış ve bitkisel insanlar dönüşmülerdi, bitkisel insanlar ataların yarattığı Dark Whether'a karşı savaşmakta ama sayıca yetersiz kalmaktadır. Bitkisel İnsanlar aralarınadaki en iyi savaşçının klonlamak istiyorlar. Bu yüzden ellerinde en iyi savaşçıyı prototip olarak kullanmak istiyecekler. 
> *Prototip olarak kullandıkları savaşçıdan birebir aynı savaşçı klonlamak istiyecekler*.

Basitçe : 
> Prototip tasarım kalıbı eldeki nesne ile benzer bir nesne yaratmak için kullanılır.

```csharp
public class SavasciPrototype
{
    internal int Can { get; set; }
    internal int Zeka { get; set; }
    internal int Guc { get; set; }
    internal int FotosentezMiktari { get; set; }
    public SavasciPrototype()
    {
    }
    public static SavasciPrototype Initialize(int can, int zeka, int guc, int fotosentezMiktari)
    {
        var savasci = new SavasciPrototype();
        savasci.Can = can;
        savasci.Zeka= zeka;
        savasci.Guc = guc;
        savasci.FotosentezMiktari = fotosentezMiktari;
        return savasci;
    }
    private static SavasciPrototype _bestWarrior = Initialize(int.MaxValue,int.MaxValue,int.MaxValue,int.MaxValue);

	private static SavasciPrototype _normalWarrior = Initialize(int.MaxValue/2,int.MaxValue/2,int.MaxValue/2,int.MaxValue/2);

    private static SavasciPrototype _badWarrior = Initialize(int.MaxValue/4,int.MaxValue/4,int.MaxValue/4,int.MaxValue/4);

    public void OzellikYazdir(string cloneName)
    {
        System.Console.WriteLine(cloneName);
        System.Console.WriteLine("{0} Guc : " + this.Guc, cloneName);
        System.Console.WriteLine("{0} Can : " + this.Can, cloneName);
        System.Console.WriteLine("{0} Zeka : " + this.Zeka, cloneName);
        System.Console.WriteLine("{0} Zeka : " + this.FotosentezMiktari, cloneName);
        System.Console.WriteLine("----------------------");

    }
    private SavasciPrototype Clone()
    {
        return (SavasciPrototype) this.MemberwiseClone();
    }

    public SavasciPrototype CloneBestWarrior()
    {
        return _bestWarrior.Clone();
    }

    public SavasciPrototype CloneNnormalWarrior()
    {
        return _normalWarrior.Clone();
    }

    public SavasciPrototype CloneBadWarrior()
    {
        return _badWarrior.Clone();
    }

}

```
Kullanımı : 
```csharp

SavasciPrototype savasci = new SavasciPrototype();

var clone1 = savasci.CloneBestWarrior();
clone1.Can =0 ; // öldü
clone1.OzellikYazdir("clone1");

var clone2 = savasci.CloneBestWarrior();
clone2.Guc = 0; // silahını düşürdü
clone2.OzellikYazdir("clone2");

var clone3 = savasci.CloneBestWarrior();
clone3.FotosentezMiktari = 0; // gereksiz insan, carbon notr
clone3.OzellikYazdir("clone3");
```


**Ne zaman kullanılmalı ?**
Bir obje yaratmak istediğimizde eğer var olan genel(general) bir statü'den türetilmek istenirse, var olan bir objeyi klonlamak bizi ekstra efordan kurtatır. Aynı zamanda objeyi yaratmanın var olan obje klonlamak daha maliyetli olduğunda da prototip kalıbı kullanılabilir.


💍 Tekil (Singleton)
------------
Gerçek Dünya Senaryosu : 
>  Dünya üzerinde süperman'den sadece ama sadece bir tane var. Süperman burada tekil'dir.

Basitçe : 
> Tekil(singleton) tasarım kalıbı bir nesnenin sadece ama sadece tek bir örneğinin olabileceği söyleyen nesnedir.

Dikkat :
> Tekil tasarım kalıbı genel olarak anti-pattern olarak anılır. Olabildiğince kullanımından kaçınılması gerekmektedir, bunun sebebi uygulamanın(application'ının) her yerinde tanımlıdır ve bu tekil nesnenin değişimi uygulamanın her yerini etkileyecektir.  Aynı zamanda global olarak ulaşılabilen nesneler debug edilmesi zordur. 
> Tekil (Singleton) tasarım kalıbının bir diğer dezantajlarından *uygulamadaki diğer kod boklarının* tekil(singleton) yapıya sıkı bağımlılık içermesi olabilir. 
>  Tekil tasarım kalıbının bir diğer dezavantajlarından biri ise kod taklit etme(mocking) işleminin zorlu olmasıdır.

**Yazılım Örneği :** 

Öncelikle süperman sınıfımızı yaratalım :
> Not : Multihread(çok kanallı) kullanımında aşağıdaki kod bloğu istendiği gibi çalışmayabilir. Bu sebep ile burayı anladıktan sonra ikinci Superman class'ı örneğinin geçrek dünya kullanımını daha uygun olacaktır!!

```csharp

public class Superman
{
    private static Superman _superman;
    private static string _name;

    private Superman(string name)
    {
        _name = name;
    }
    public static Superman GetInstance()
    {
       
		if (_superman == null)
		{
			_superman = new Superman("Clark Kent");
		}
        return _superman;
    }
    public void PrintSupermanName()
    {
        Console.WriteLine(_name);
    }
}
```

Client Kodu aşağıdadır : 
```csharp
var superman =Superman.GetInstance();
superman.PrintSupermanName();
```

*Superman sınıfını multithread safe (çok kanallı yapı için güvenli) hale  getirelim, * **client kodu hala yukarıdaki gibidir.** 

```csharp

public class Superman
{
    private static Superman _superman;
    private static string _name;

    private static readonly object dummy = new Object(); // dummy lock object eklendi.
    
    private Superman(string name)
    {
        _name = name;
    }
    public static Superman GetInstance()
    {
	    // double null checking ve lock eklendi
        if (_superman == null)
        {
            lock (dummy)
            {
                if (_superman == null)
                {
                    _superman = new Superman("Clark Kent");
                }
            }
        }
        return _superman;
    }
    public void PrintSupermanName()
    {
        Console.WriteLine(_name);
    }
}
```

**Ne zaman kullanılır** ?
Tekil tasarım kalıbı yani singleton bir nesnenin uygulama hayatı boyunca yalnız ve yalnız bir tane örneği(instance) olmasını istediğimiz zaman kullanırız. Ama kullanımından kaçınılması gereken bir tasarım kalıbıdır.

Yapısal Tasarım Kalıpları
==========================
Basitçe : 
> Yapısal Tasarım Kalıpları yazılım bileşenleri bir araya gelerek daha kompleks bileşenleri oluşturmak için kullanlır.

 * [Adaptör](#adaptöradapter)
 * [Bridge](#-bridge)
 * [Composite](#-composite)
 * [Decorator](#-decorator)
 * [Facade](#-facade)
 * [Flyweight](#-flyweight)
 * [Proxy](#-proxy)

🔌Adaptör(Adapter)
-------

Gerçek dünya senaryosu :
> Diyelimki bir tane tablet şarj cihazınız var ve şarj yeri Çin'deki prizlere uygun olsun ama biz Türkiye'de yaşadığımızdan Çin tipi şarj cihazının uyum sağlayacağı bir prizimiz yok bunun için priz dönüştürüceye ihtiyacımız var, bu priz dönüştürücü bizim adaptörümüzdür.

Basitçe :
> Adaptör tasarım kalıbı : Birbirleriyle uyumsuz yapıları beraber kullanmamıza olanak sağlar.

Yazılım Örneğin
> Diyelimki bir prizimiz var Tükiyede normal bir şekilde kullanılan bir priz yuvarlak demir uç girişine uyumlu olanlardan. Biz buna çin tipi girişe sahip bir şarj cihazı takamayız bu sebeple araya bir SarjCihaziAdaptor koymamız gerekiyor.

```csharp
public class Program
{
	public static void Main()
	{
		var priz = new Priz();
		var cinTipiSarj = new CinSarjCihaz();
		var adaptor = new SarjCihaziAdaptor(cinTipiSarj);
		priz.ElektrikIlet(adaptor);
	}
}

public interface ICinTipiSarj 
{
	void SarjEt();
}

public interface ISarj 
{
	 void SarjEt();
}

public class SarjCihazi : ISarj 
{
	public void SarjEt() 
	{
		Console.WriteLine("Şarj Oluyor");
	}
}

public class CinSarjCihaz : ICinTipiSarj 
{
	public void SarjEt()
	{
		Console.WriteLine("Çin tipi Şarj Oluyor");
	}
}

public class Priz
{
	public void ElektrikIlet(ISarj sarj) 
	{
		sarj.SarjEt();
	}
}

class SarjCihaziAdaptor : ISarj
{
	private readonly ICinTipiSarj _cinSarjCihazi;
	public SarjCihaziAdaptor(ICinTipiSarj cinSarjCihazi)
	{
		_cinSarjCihazi = cinSarjCihazi;
	}
	public void SarjEt()
	{
		_cinSarjCihazi.SarjEt();
	}
}
```



🚡 Bridge
------
Real world example
> Consider you have a website with different pages and you are supposed to allow the user to change the theme. What would you do? Create multiple copies of each of the pages for each of the themes or would you just create separate theme and load them based on the user's preferences? Bridge pattern allows you to do the second i.e.

![With and without the bridge pattern](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png)

In Plain Words
> Bridge pattern is about preferring composition over inheritance. Implementation details are pushed from a hierarchy to another object with a separate hierarchy.

Wikipedia says
> The bridge pattern is a design pattern used in software engineering that is meant to "decouple an abstraction from its implementation so that the two can vary independently"

**Programmatic Example**

Translating our WebPage example from above. Here we have the `WebPage` hierarchy

```php
interface WebPage
{
    public function __construct(Theme $theme);
    public function getContent();
}

class About implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "About page in " . $this->theme->getColor();
    }
}

class Careers implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "Careers page in " . $this->theme->getColor();
    }
}
```
And the separate theme hierarchy
```php

interface Theme
{
    public function getColor();
}

class DarkTheme implements Theme
{
    public function getColor()
    {
        return 'Dark Black';
    }
}
class LightTheme implements Theme
{
    public function getColor()
    {
        return 'Off white';
    }
}
class AquaTheme implements Theme
{
    public function getColor()
    {
        return 'Light blue';
    }
}
```
And both the hierarchies
```php
$darkTheme = new DarkTheme();

$about = new About($darkTheme);
$careers = new Careers($darkTheme);

echo $about->getContent(); // "About page in Dark Black";
echo $careers->getContent(); // "Careers page in Dark Black";
```

🌿 Composite
-----------------

Real world example
> Every organization is composed of employees. Each of the employees has the same features i.e. has a salary, has some responsibilities, may or may not report to someone, may or may not have some subordinates etc.

In plain words
> Composite pattern lets clients treat the individual objects in a uniform manner.

Wikipedia says
> In software engineering, the composite pattern is a partitioning design pattern. The composite pattern describes that a group of objects is to be treated in the same way as a single instance of an object. The intent of a composite is to "compose" objects into tree structures to represent part-whole hierarchies. Implementing the composite pattern lets clients treat individual objects and compositions uniformly.

**Programmatic Example**

Taking our employees example from above. Here we have different employee types

```php
interface Employee
{
    public function __construct(string $name, float $salary);
    public function getName(): string;
    public function setSalary(float $salary);
    public function getSalary(): float;
    public function getRoles(): array;
}

class Developer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;
    
    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}

class Designer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;

    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}
```

Then we have an organization which consists of several different types of employees

```php
class Organization
{
    protected $employees;

    public function addEmployee(Employee $employee)
    {
        $this->employees[] = $employee;
    }

    public function getNetSalaries(): float
    {
        $netSalary = 0;

        foreach ($this->employees as $employee) {
            $netSalary += $employee->getSalary();
        }

        return $netSalary;
    }
}
```

And then it can be used as

```php
// Prepare the employees
$john = new Developer('John Doe', 12000);
$jane = new Designer('Jane Doe', 15000);

// Add them to organization
$organization = new Organization();
$organization->addEmployee($john);
$organization->addEmployee($jane);

echo "Net salaries: " . $organization->getNetSalaries(); // Net Salaries: 27000
```

☕ Decorator
-------------

Real world example

> Imagine you run a car service shop offering multiple services. Now how do you calculate the bill to be charged? You pick one service and dynamically keep adding to it the prices for the provided services till you get the final cost. Here each type of service is a decorator.

In plain words
> Decorator pattern lets you dynamically change the behavior of an object at run time by wrapping them in an object of a decorator class.

Wikipedia says
> In object-oriented programming, the decorator pattern is a design pattern that allows behavior to be added to an individual object, either statically or dynamically, without affecting the behavior of other objects from the same class. The decorator pattern is often useful for adhering to the Single Responsibility Principle, as it allows functionality to be divided between classes with unique areas of concern.

**Programmatic Example**

Lets take coffee for example. First of all we have a simple coffee implementing the coffee interface

```php
interface Coffee
{
    public function getCost();
    public function getDescription();
}

class SimpleCoffee implements Coffee
{
    public function getCost()
    {
        return 10;
    }

    public function getDescription()
    {
        return 'Simple coffee';
    }
}
```
We want to make the code extensible to allow options to modify it if required. Lets make some add-ons (decorators)
```php
class MilkCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 2;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', milk';
    }
}

class WhipCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 5;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', whip';
    }
}

class VanillaCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 3;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', vanilla';
    }
}
```

Lets make a coffee now

```php
$someCoffee = new SimpleCoffee();
echo $someCoffee->getCost(); // 10
echo $someCoffee->getDescription(); // Simple Coffee

$someCoffee = new MilkCoffee($someCoffee);
echo $someCoffee->getCost(); // 12
echo $someCoffee->getDescription(); // Simple Coffee, milk

$someCoffee = new WhipCoffee($someCoffee);
echo $someCoffee->getCost(); // 17
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip

$someCoffee = new VanillaCoffee($someCoffee);
echo $someCoffee->getCost(); // 20
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip, vanilla
```

📦 Facade
----------------

Real world example
> How do you turn on the computer? "Hit the power button" you say! That is what you believe because you are using a simple interface that computer provides on the outside, internally it has to do a lot of stuff to make it happen. This simple interface to the complex subsystem is a facade.

In plain words
> Facade pattern provides a simplified interface to a complex subsystem.

Wikipedia says
> A facade is an object that provides a simplified interface to a larger body of code, such as a class library.

**Programmatic Example**

Taking our computer example from above. Here we have the computer class

```php
class Computer
{
    public function getElectricShock()
    {
        echo "Ouch!";
    }

    public function makeSound()
    {
        echo "Beep beep!";
    }

    public function showLoadingScreen()
    {
        echo "Loading..";
    }

    public function bam()
    {
        echo "Ready to be used!";
    }

    public function closeEverything()
    {
        echo "Bup bup bup buzzzz!";
    }

    public function sooth()
    {
        echo "Zzzzz";
    }

    public function pullCurrent()
    {
        echo "Haaah!";
    }
}
```
Here we have the facade
```php
class ComputerFacade
{
    protected $computer;

    public function __construct(Computer $computer)
    {
        $this->computer = $computer;
    }

    public function turnOn()
    {
        $this->computer->getElectricShock();
        $this->computer->makeSound();
        $this->computer->showLoadingScreen();
        $this->computer->bam();
    }

    public function turnOff()
    {
        $this->computer->closeEverything();
        $this->computer->pullCurrent();
        $this->computer->sooth();
    }
}
```
Now to use the facade
```php
$computer = new ComputerFacade(new Computer());
$computer->turnOn(); // Ouch! Beep beep! Loading.. Ready to be used!
$computer->turnOff(); // Bup bup buzzz! Haah! Zzzzz
```

🍃 Flyweight
---------

Real world example
> Did you ever have fresh tea from some stall? They often make more than one cup that you demanded and save the rest for any other customer so to save the resources e.g. gas etc. Flyweight pattern is all about that i.e. sharing.

In plain words
> It is used to minimize memory usage or computational expenses by sharing as much as possible with similar objects.

Wikipedia says
> In computer programming, flyweight is a software design pattern. A flyweight is an object that minimizes memory use by sharing as much data as possible with other similar objects; it is a way to use objects in large numbers when a simple repeated representation would use an unacceptable amount of memory.

**Programmatic example**

Translating our tea example from above. First of all we have tea types and tea maker

```php
// Anything that will be cached is flyweight.
// Types of tea here will be flyweights.
class KarakTea
{
}

// Acts as a factory and saves the tea
class TeaMaker
{
    protected $availableTea = [];

    public function make($preference)
    {
        if (empty($this->availableTea[$preference])) {
            $this->availableTea[$preference] = new KarakTea();
        }

        return $this->availableTea[$preference];
    }
}
```

Then we have the `TeaShop` which takes orders and serves them

```php
class TeaShop
{
    protected $orders;
    protected $teaMaker;

    public function __construct(TeaMaker $teaMaker)
    {
        $this->teaMaker = $teaMaker;
    }

    public function takeOrder(string $teaType, int $table)
    {
        $this->orders[$table] = $this->teaMaker->make($teaType);
    }

    public function serve()
    {
        foreach ($this->orders as $table => $tea) {
            echo "Serving tea to table# " . $table;
        }
    }
}
```
And it can be used as below

```php
$teaMaker = new TeaMaker();
$shop = new TeaShop($teaMaker);

$shop->takeOrder('less sugar', 1);
$shop->takeOrder('more milk', 2);
$shop->takeOrder('without sugar', 5);

$shop->serve();
// Serving tea to table# 1
// Serving tea to table# 2
// Serving tea to table# 5
```

🎱 Proxy
-------------------
Real world example
> Have you ever used an access card to go through a door? There are multiple options to open that door i.e. it can be opened either using access card or by pressing a button that bypasses the security. The door's main functionality is to open but there is a proxy added on top of it to add some functionality. Let me better explain it using the code example below.

In plain words
> Using the proxy pattern, a class represents the functionality of another class.

Wikipedia says
> A proxy, in its most general form, is a class functioning as an interface to something else. A proxy is a wrapper or agent object that is being called by the client to access the real serving object behind the scenes. Use of the proxy can simply be forwarding to the real object, or can provide additional logic. In the proxy extra functionality can be provided, for example caching when operations on the real object are resource intensive, or checking preconditions before operations on the real object are invoked.

**Programmatic Example**

Taking our security door example from above. Firstly we have the door interface and an implementation of door

```php
interface Door
{
    public function open();
    public function close();
}

class LabDoor implements Door
{
    public function open()
    {
        echo "Opening lab door";
    }

    public function close()
    {
        echo "Closing the lab door";
    }
}
```
Then we have a proxy to secure any doors that we want
```php
class SecuredDoor implements Door
{
    protected $door;

    public function __construct(Door $door)
    {
        $this->door = $door;
    }

    public function open($password)
    {
        if ($this->authenticate($password)) {
            $this->door->open();
        } else {
            echo "Big no! It ain't possible.";
        }
    }

    public function authenticate($password)
    {
        return $password === '$ecr@t';
    }

    public function close()
    {
        $this->door->close();
    }
}
```
And here is how it can be used
```php
$door = new SecuredDoor(new LabDoor());
$door->open('invalid'); // Big no! It ain't possible.

$door->open('$ecr@t'); // Opening lab door
$door->close(); // Closing lab door
```
Yet another example would be some sort of data-mapper implementation. For example, I recently made an ODM (Object Data Mapper) for MongoDB using this pattern where I wrote a proxy around mongo classes while utilizing the magic method `__call()`. All the method calls were proxied to the original mongo class and result retrieved was returned as it is but in case of `find` or `findOne` data was mapped to the required class objects and the object was returned instead of `Cursor`.

Behavioral Design Patterns
==========================

In plain words
> It is concerned with assignment of responsibilities between the objects. What makes them different from structural patterns is they don't just specify the structure but also outline the patterns for message passing/communication between them. Or in other words, they assist in answering "How to run a behavior in software component?"

Wikipedia says
> In software engineering, behavioral design patterns are design patterns that identify common communication patterns between objects and realize these patterns. By doing so, these patterns increase flexibility in carrying out this communication.

* [Chain of Responsibility](#-chain-of-responsibility)
* [Command](#-command)
* [Iterator](#-iterator)
* [Mediator](#-mediator)
* [Memento](#-memento)
* [Observer](#-observer)
* [Visitor](#-visitor)
* [Strategy](#-strategy)
* [State](#-state)
* [Template Method](#-template-method)

🔗 Chain of Responsibility
-----------------------

Real world example
> For example, you have three payment methods (`A`, `B` and `C`) setup in your account; each having a different amount in it. `A` has 100 USD, `B` has 300 USD and `C` having 1000 USD and the preference for payments is chosen as `A` then `B` then `C`. You try to purchase something that is worth 210 USD. Using Chain of Responsibility, first of all account `A` will be checked if it can make the purchase, if yes purchase will be made and the chain will be broken. If not, request will move forward to account `B` checking for amount if yes chain will be broken otherwise the request will keep forwarding till it finds the suitable handler. Here `A`, `B` and `C` are links of the chain and the whole phenomenon is Chain of Responsibility.

In plain words
> It helps building a chain of objects. Request enters from one end and keeps going from object to object till it finds the suitable handler.

Wikipedia says
> In object-oriented design, the chain-of-responsibility pattern is a design pattern consisting of a source of command objects and a series of processing objects. Each processing object contains logic that defines the types of command objects that it can handle; the rest are passed to the next processing object in the chain.

**Programmatic Example**

Translating our account example above. First of all we have a base account having the logic for chaining the accounts together and some accounts

```php
abstract class Account
{
    protected $successor;
    protected $balance;

    public function setNext(Account $account)
    {
        $this->successor = $account;
    }

    public function pay(float $amountToPay)
    {
        if ($this->canPay($amountToPay)) {
            echo sprintf('Paid %s using %s' . PHP_EOL, $amountToPay, get_called_class());
        } elseif ($this->successor) {
            echo sprintf('Cannot pay using %s. Proceeding ..' . PHP_EOL, get_called_class());
            $this->successor->pay($amountToPay);
        } else {
            throw new Exception('None of the accounts have enough balance');
        }
    }

    public function canPay($amount): bool
    {
        return $this->balance >= $amount;
    }
}

class Bank extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}

class Paypal extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}

class Bitcoin extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}
```

Now let's prepare the chain using the links defined above (i.e. Bank, Paypal, Bitcoin)

```php
// Let's prepare a chain like below
//      $bank->$paypal->$bitcoin
//
// First priority bank
//      If bank can't pay then paypal
//      If paypal can't pay then bit coin

$bank = new Bank(100);          // Bank with balance 100
$paypal = new Paypal(200);      // Paypal with balance 200
$bitcoin = new Bitcoin(300);    // Bitcoin with balance 300

$bank->setNext($paypal);
$paypal->setNext($bitcoin);

// Let's try to pay using the first priority i.e. bank
$bank->pay(259);

// Output will be
// ==============
// Cannot pay using bank. Proceeding ..
// Cannot pay using paypal. Proceeding ..:
// Paid 259 using Bitcoin!
```

👮 Command
-------

Real world example
> A generic example would be you ordering food at a restaurant. You (i.e. `Client`) ask the waiter (i.e. `Invoker`) to bring some food (i.e. `Command`) and waiter simply forwards the request to Chef (i.e. `Receiver`) who has the knowledge of what and how to cook.
> Another example would be you (i.e. `Client`) switching on (i.e. `Command`) the television (i.e. `Receiver`) using a remote control (`Invoker`).

In plain words
> Allows you to encapsulate actions in objects. The key idea behind this pattern is to provide the means to decouple client from receiver.

Wikipedia says
> In object-oriented programming, the command pattern is a behavioral design pattern in which an object is used to encapsulate all information needed to perform an action or trigger an event at a later time. This information includes the method name, the object that owns the method and values for the method parameters.

**Programmatic Example**

First of all we have the receiver that has the implementation of every action that could be performed
```php
// Receiver
class Bulb
{
    public function turnOn()
    {
        echo "Bulb has been lit";
    }

    public function turnOff()
    {
        echo "Darkness!";
    }
}
```
then we have an interface that each of the commands are going to implement and then we have a set of commands
```php
interface Command
{
    public function execute();
    public function undo();
    public function redo();
}

// Command
class TurnOn implements Command
{
    protected $bulb;

    public function __construct(Bulb $bulb)
    {
        $this->bulb = $bulb;
    }

    public function execute()
    {
        $this->bulb->turnOn();
    }

    public function undo()
    {
        $this->bulb->turnOff();
    }

    public function redo()
    {
        $this->execute();
    }
}

class TurnOff implements Command
{
    protected $bulb;

    public function __construct(Bulb $bulb)
    {
        $this->bulb = $bulb;
    }

    public function execute()
    {
        $this->bulb->turnOff();
    }

    public function undo()
    {
        $this->bulb->turnOn();
    }

    public function redo()
    {
        $this->execute();
    }
}
```
Then we have an `Invoker` with whom the client will interact to process any commands
```php
// Invoker
class RemoteControl
{
    public function submit(Command $command)
    {
        $command->execute();
    }
}
```
Finally let's see how we can use it in our client
```php
$bulb = new Bulb();

$turnOn = new TurnOn($bulb);
$turnOff = new TurnOff($bulb);

$remote = new RemoteControl();
$remote->submit($turnOn); // Bulb has been lit!
$remote->submit($turnOff); // Darkness!
```

Command pattern can also be used to implement a transaction based system. Where you keep maintaining the history of commands as soon as you execute them. If the final command is successfully executed, all good otherwise just iterate through the history and keep executing the `undo` on all the executed commands.

➿ Iterator
--------

Real world example
> An old radio set will be a good example of iterator, where user could start at some channel and then use next or previous buttons to go through the respective channels. Or take an example of MP3 player or a TV set where you could press the next and previous buttons to go through the consecutive channels or in other words they all provide an interface to iterate through the respective channels, songs or radio stations.  

In plain words
> It presents a way to access the elements of an object without exposing the underlying presentation.

Wikipedia says
> In object-oriented programming, the iterator pattern is a design pattern in which an iterator is used to traverse a container and access the container's elements. The iterator pattern decouples algorithms from containers; in some cases, algorithms are necessarily container-specific and thus cannot be decoupled.

**Programmatic example**

In PHP it is quite easy to implement using SPL (Standard PHP Library). Translating our radio stations example from above. First of all we have `RadioStation`

```php
class RadioStation
{
    protected $frequency;

    public function __construct(float $frequency)
    {
        $this->frequency = $frequency;
    }

    public function getFrequency(): float
    {
        return $this->frequency;
    }
}
```
Then we have our iterator

```php
use Countable;
use Iterator;

class StationList implements Countable, Iterator
{
    /** @var RadioStation[] $stations */
    protected $stations = [];

    /** @var int $counter */
    protected $counter;

    public function addStation(RadioStation $station)
    {
        $this->stations[] = $station;
    }

    public function removeStation(RadioStation $toRemove)
    {
        $toRemoveFrequency = $toRemove->getFrequency();
        $this->stations = array_filter($this->stations, function (RadioStation $station) use ($toRemoveFrequency) {
            return $station->getFrequency() !== $toRemoveFrequency;
        });
    }

    public function count(): int
    {
        return count($this->stations);
    }

    public function current(): RadioStation
    {
        return $this->stations[$this->counter];
    }

    public function key()
    {
        return $this->counter;
    }

    public function next()
    {
        $this->counter++;
    }

    public function rewind()
    {
        $this->counter = 0;
    }

    public function valid(): bool
    {
        return isset($this->stations[$this->counter]);
    }
}
```
And then it can be used as
```php
$stationList = new StationList();

$stationList->addStation(new RadioStation(89));
$stationList->addStation(new RadioStation(101));
$stationList->addStation(new RadioStation(102));
$stationList->addStation(new RadioStation(103.2));

foreach($stationList as $station) {
    echo $station->getFrequency() . PHP_EOL;
}

$stationList->removeStation(new RadioStation(89)); // Will remove station 89
```

👽 Mediator
========

Real world example
> A general example would be when you talk to someone on your mobile phone, there is a network provider sitting between you and them and your conversation goes through it instead of being directly sent. In this case network provider is mediator.

In plain words
> Mediator pattern adds a third party object (called mediator) to control the interaction between two objects (called colleagues). It helps reduce the coupling between the classes communicating with each other. Because now they don't need to have the knowledge of each other's implementation.

Wikipedia says
> In software engineering, the mediator pattern defines an object that encapsulates how a set of objects interact. This pattern is considered to be a behavioral pattern due to the way it can alter the program's running behavior.

**Programmatic Example**

Here is the simplest example of a chat room (i.e. mediator) with users (i.e. colleagues) sending messages to each other.

First of all, we have the mediator i.e. the chat room

```php
interface ChatRoomMediator 
{
    public function showMessage(User $user, string $message);
}

// Mediator
class ChatRoom implements ChatRoomMediator
{
    public function showMessage(User $user, string $message)
    {
        $time = date('M d, y H:i');
        $sender = $user->getName();

        echo $time . '[' . $sender . ']:' . $message;
    }
}
```

Then we have our users i.e. colleagues
```php
class User {
    protected $name;
    protected $chatMediator;

    public function __construct(string $name, ChatRoomMediator $chatMediator) {
        $this->name = $name;
        $this->chatMediator = $chatMediator;
    }

    public function getName() {
        return $this->name;
    }

    public function send($message) {
        $this->chatMediator->showMessage($this, $message);
    }
}
```
And the usage
```php
$mediator = new ChatRoom();

$john = new User('John Doe', $mediator);
$jane = new User('Jane Doe', $mediator);

$john->send('Hi there!');
$jane->send('Hey!');

// Output will be
// Feb 14, 10:58 [John]: Hi there!
// Feb 14, 10:58 [Jane]: Hey!
```

💾 Memento
-------
Real world example
> Take the example of calculator (i.e. originator), where whenever you perform some calculation the last calculation is saved in memory (i.e. memento) so that you can get back to it and maybe get it restored using some action buttons (i.e. caretaker).

In plain words
> Memento pattern is about capturing and storing the current state of an object in a manner that it can be restored later on in a smooth manner.

Wikipedia says
> The memento pattern is a software design pattern that provides the ability to restore an object to its previous state (undo via rollback).

Usually useful when you need to provide some sort of undo functionality.

**Programmatic Example**

Lets take an example of text editor which keeps saving the state from time to time and that you can restore if you want.

First of all we have our memento object that will be able to hold the editor state

```php
class EditorMemento
{
    protected $content;

    public function __construct(string $content)
    {
        $this->content = $content;
    }

    public function getContent()
    {
        return $this->content;
    }
}
```

Then we have our editor i.e. originator that is going to use memento object

```php
class Editor
{
    protected $content = '';

    public function type(string $words)
    {
        $this->content = $this->content . ' ' . $words;
    }

    public function getContent()
    {
        return $this->content;
    }

    public function save()
    {
        return new EditorMemento($this->content);
    }

    public function restore(EditorMemento $memento)
    {
        $this->content = $memento->getContent();
    }
}
```

And then it can be used as

```php
$editor = new Editor();

// Type some stuff
$editor->type('This is the first sentence.');
$editor->type('This is second.');

// Save the state to restore to : This is the first sentence. This is second.
$saved = $editor->save();

// Type some more
$editor->type('And this is third.');

// Output: Content before Saving
echo $editor->getContent(); // This is the first sentence. This is second. And this is third.

// Restoring to last saved state
$editor->restore($saved);

$editor->getContent(); // This is the first sentence. This is second.
```

😎 Observer
--------
Real world example
> A good example would be the job seekers where they subscribe to some job posting site and they are notified whenever there is a matching job opportunity.   

In plain words
> Defines a dependency between objects so that whenever an object changes its state, all its dependents are notified.

Wikipedia says
> The observer pattern is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods.

**Programmatic example**

Translating our example from above. First of all we have job seekers that need to be notified for a job posting
```php
class JobPost
{
    protected $title;

    public function __construct(string $title)
    {
        $this->title = $title;
    }

    public function getTitle()
    {
        return $this->title;
    }
}

class JobSeeker implements Observer
{
    protected $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function onJobPosted(JobPost $job)
    {
        // Do something with the job posting
        echo 'Hi ' . $this->name . '! New job posted: '. $job->getTitle();
    }
}
```
Then we have our job postings to which the job seekers will subscribe
```php
class EmploymentAgency implements Observable
{
    protected $observers = [];

    protected function notify(JobPost $jobPosting)
    {
        foreach ($this->observers as $observer) {
            $observer->onJobPosted($jobPosting);
        }
    }

    public function attach(Observer $observer)
    {
        $this->observers[] = $observer;
    }

    public function addJob(JobPost $jobPosting)
    {
        $this->notify($jobPosting);
    }
}
```
Then it can be used as
```php
// Create subscribers
$johnDoe = new JobSeeker('John Doe');
$janeDoe = new JobSeeker('Jane Doe');

// Create publisher and attach subscribers
$jobPostings = new EmploymentAgency();
$jobPostings->attach($johnDoe);
$jobPostings->attach($janeDoe);

// Add a new job and see if subscribers get notified
$jobPostings->addJob(new JobPost('Software Engineer'));

// Output
// Hi John Doe! New job posted: Software Engineer
// Hi Jane Doe! New job posted: Software Engineer
```

🏃 Visitor
-------
Real world example
> Consider someone visiting Dubai. They just need a way (i.e. visa) to enter Dubai. After arrival, they can come and visit any place in Dubai on their own without having to ask for permission or to do some leg work in order to visit any place here; just let them know of a place and they can visit it. Visitor pattern lets you do just that, it helps you add places to visit so that they can visit as much as they can without having to do any legwork.

In plain words
> Visitor pattern lets you add further operations to objects without having to modify them.

Wikipedia says
> In object-oriented programming and software engineering, the visitor design pattern is a way of separating an algorithm from an object structure on which it operates. A practical result of this separation is the ability to add new operations to existing object structures without modifying those structures. It is one way to follow the open/closed principle.

**Programmatic example**

Let's take an example of a zoo simulation where we have several different kinds of animals and we have to make them Sound. Let's translate this using visitor pattern

```php
// Visitee
interface Animal
{
    public function accept(AnimalOperation $operation);
}

// Visitor
interface AnimalOperation
{
    public function visitMonkey(Monkey $monkey);
    public function visitLion(Lion $lion);
    public function visitDolphin(Dolphin $dolphin);
}
```
Then we have our implementations for the animals
```php
class Monkey implements Animal
{
    public function shout()
    {
        echo 'Ooh oo aa aa!';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitMonkey($this);
    }
}

class Lion implements Animal
{
    public function roar()
    {
        echo 'Roaaar!';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitLion($this);
    }
}

class Dolphin implements Animal
{
    public function speak()
    {
        echo 'Tuut tuttu tuutt!';
    }

    public function accept(AnimalOperation $operation)
    {
        $operation->visitDolphin($this);
    }
}
```
Let's implement our visitor
```php
class Speak implements AnimalOperation
{
    public function visitMonkey(Monkey $monkey)
    {
        $monkey->shout();
    }

    public function visitLion(Lion $lion)
    {
        $lion->roar();
    }

    public function visitDolphin(Dolphin $dolphin)
    {
        $dolphin->speak();
    }
}
```

And then it can be used as
```php
$monkey = new Monkey();
$lion = new Lion();
$dolphin = new Dolphin();

$speak = new Speak();

$monkey->accept($speak);    // Ooh oo aa aa!    
$lion->accept($speak);      // Roaaar!
$dolphin->accept($speak);   // Tuut tutt tuutt!
```
We could have done this simply by having an inheritance hierarchy for the animals but then we would have to modify the animals whenever we would have to add new actions to animals. But now we will not have to change them. For example, let's say we are asked to add the jump behavior to the animals, we can simply add that by creating a new visitor i.e.

```php
class Jump implements AnimalOperation
{
    public function visitMonkey(Monkey $monkey)
    {
        echo 'Jumped 20 feet high! on to the tree!';
    }

    public function visitLion(Lion $lion)
    {
        echo 'Jumped 7 feet! Back on the ground!';
    }

    public function visitDolphin(Dolphin $dolphin)
    {
        echo 'Walked on water a little and disappeared';
    }
}
```
And for the usage
```php
$jump = new Jump();

$monkey->accept($speak);   // Ooh oo aa aa!
$monkey->accept($jump);    // Jumped 20 feet high! on to the tree!

$lion->accept($speak);     // Roaaar!
$lion->accept($jump);      // Jumped 7 feet! Back on the ground!

$dolphin->accept($speak);  // Tuut tutt tuutt!
$dolphin->accept($jump);   // Walked on water a little and disappeared
```

💡 Strategy
--------

Real world example
> Consider the example of sorting, we implemented bubble sort but the data started to grow and bubble sort started getting very slow. In order to tackle this we implemented Quick sort. But now although the quick sort algorithm was doing better for large datasets, it was very slow for smaller datasets. In order to handle this we implemented a strategy where for small datasets, bubble sort will be used and for larger, quick sort.

In plain words
> Strategy pattern allows you to switch the algorithm or strategy based upon the situation.

Wikipedia says
> In computer programming, the strategy pattern (also known as the policy pattern) is a behavioural software design pattern that enables an algorithm's behavior to be selected at runtime.

**Programmatic example**

Translating our example from above. First of all we have our strategy interface and different strategy implementations

```php
interface SortStrategy
{
    public function sort(array $dataset): array;
}

class BubbleSortStrategy implements SortStrategy
{
    public function sort(array $dataset): array
    {
        echo "Sorting using bubble sort";

        // Do sorting
        return $dataset;
    }
}

class QuickSortStrategy implements SortStrategy
{
    public function sort(array $dataset): array
    {
        echo "Sorting using quick sort";

        // Do sorting
        return $dataset;
    }
}
```

And then we have our client that is going to use any strategy
```php
class Sorter
{
    protected $sorter;

    public function __construct(SortStrategy $sorter)
    {
        $this->sorter = $sorter;
    }

    public function sort(array $dataset): array
    {
        return $this->sorter->sort($dataset);
    }
}
```
And it can be used as
```php
$dataset = [1, 5, 4, 3, 2, 8];

$sorter = new Sorter(new BubbleSortStrategy());
$sorter->sort($dataset); // Output : Sorting using bubble sort

$sorter = new Sorter(new QuickSortStrategy());
$sorter->sort($dataset); // Output : Sorting using quick sort
```

💢 State
-----
Real world example
> Imagine you are using some drawing application, you choose the paint brush to draw. Now the brush changes its behavior based on the selected color i.e. if you have chosen red color it will draw in red, if blue then it will be in blue etc.  

In plain words
> It lets you change the behavior of a class when the state changes.

Wikipedia says
> The state pattern is a behavioral software design pattern that implements a state machine in an object-oriented way. With the state pattern, a state machine is implemented by implementing each individual state as a derived class of the state pattern interface, and implementing state transitions by invoking methods defined by the pattern's superclass.
> The state pattern can be interpreted as a strategy pattern which is able to switch the current strategy through invocations of methods defined in the pattern's interface.

**Programmatic example**

Let's take an example of text editor, it lets you change the state of text that is typed i.e. if you have selected bold, it starts writing in bold, if italic then in italics etc.

First of all we have our state interface and some state implementations

```php
interface WritingState
{
    public function write(string $words);
}

class UpperCase implements WritingState
{
    public function write(string $words)
    {
        echo strtoupper($words);
    }
}

class LowerCase implements WritingState
{
    public function write(string $words)
    {
        echo strtolower($words);
    }
}

class DefaultText implements WritingState
{
    public function write(string $words)
    {
        echo $words;
    }
}
```
Then we have our editor
```php
class TextEditor
{
    protected $state;

    public function __construct(WritingState $state)
    {
        $this->state = $state;
    }

    public function setState(WritingState $state)
    {
        $this->state = $state;
    }

    public function type(string $words)
    {
        $this->state->write($words);
    }
}
```
And then it can be used as
```php
$editor = new TextEditor(new DefaultText());

$editor->type('First line');

$editor->setState(new UpperCase());

$editor->type('Second line');
$editor->type('Third line');

$editor->setState(new LowerCase());

$editor->type('Fourth line');
$editor->type('Fifth line');

// Output:
// First line
// SECOND LINE
// THIRD LINE
// fourth line
// fifth line
```

📒 Template Method
---------------

Real world example
> Suppose we are getting some house built. The steps for building might look like
> - Prepare the base of house
> - Build the walls
> - Add roof
> - Add other floors

> The order of these steps could never be changed i.e. you can't build the roof before building the walls etc but each of the steps could be modified for example walls can be made of wood or polyester or stone.

In plain words
> Template method defines the skeleton of how a certain algorithm could be performed, but defers the implementation of those steps to the children classes.

Wikipedia says
> In software engineering, the template method pattern is a behavioral design pattern that defines the program skeleton of an algorithm in an operation, deferring some steps to subclasses. It lets one redefine certain steps of an algorithm without changing the algorithm's structure.

**Programmatic Example**

Imagine we have a build tool that helps us test, lint, build, generate build reports (i.e. code coverage reports, linting report etc) and deploy our app on the test server.

First of all we have our base class that specifies the skeleton for the build algorithm
```php
abstract class Builder
{

    // Template method
    final public function build()
    {
        $this->test();
        $this->lint();
        $this->assemble();
        $this->deploy();
    }

    abstract public function test();
    abstract public function lint();
    abstract public function assemble();
    abstract public function deploy();
}
```

Then we can have our implementations

```php
class AndroidBuilder extends Builder
{
    public function test()
    {
        echo 'Running android tests';
    }

    public function lint()
    {
        echo 'Linting the android code';
    }

    public function assemble()
    {
        echo 'Assembling the android build';
    }

    public function deploy()
    {
        echo 'Deploying android build to server';
    }
}

class IosBuilder extends Builder
{
    public function test()
    {
        echo 'Running ios tests';
    }

    public function lint()
    {
        echo 'Linting the ios code';
    }

    public function assemble()
    {
        echo 'Assembling the ios build';
    }

    public function deploy()
    {
        echo 'Deploying ios build to server';
    }
}
```
And then it can be used as

```php
$androidBuilder = new AndroidBuilder();
$androidBuilder->build();

// Output:
// Running android tests
// Linting the android code
// Assembling the android build
// Deploying android build to server

$iosBuilder = new IosBuilder();
$iosBuilder->build();

// Output:
// Running ios tests
// Linting the ios code
// Assembling the ios build
// Deploying ios build to server
```

## 🚦 Wrap Up Folks

And that about wraps it up. I will continue to improve this, so you might want to watch/star this repository to revisit. Also, I have plans on writing the same about the architectural patterns, stay tuned for it.

## 👬 Contribution

- Report issues
- Open pull request with improvements
- Spread the word
- Reach out with any feedback [![Twitter URL](https://img.shields.io/twitter/url/https/twitter.com/kamranahmedse.svg?style=social&label=Follow%20%40kamranahmedse)](https://twitter.com/kamranahmedse)

## License

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
