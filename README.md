Vue.js iki şekilde projeye dahil edilir.

1. Geliştirici modu: (Consolda veya sayfa yüklenirken yapılan hataların gösterildiği ve yardımcı olduğu mod)
    
    ```jsx
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    ```
    
2.  Yayın modu: (Ekranda veya consolda hata göstermez olduğu gibi çalıştrımaya çalışır)
    
    ```jsx
    <script src="https://cdn.jsdelivr.net/npm/vue@2"></script>
    ```
    

Ekrana değişken basarak başlayalım:

./HelloWorld.html

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
		<! kullanacağımız cdn'i import ediyoruz
</head>
<body>
    <div id="app"><! app id'sine sahip bir div oluşturuyoruz
        <h1>{{ title }}</h1><! değişkenimizi bu şekilde yazıyoruz
    </div>
    
</body>
<script>
    new Vue({
        el:'#app',//yapacağım işlemler app id'sine sahip eleman altında gerçekleşecek
        data:{//kullanacağım değişkenler
            title:'Hello World'
        }
    });
</script>
</html>
```

 Ekrana değişken basmak için kullanabileceğimiz bir de v-html ve v-text var. Yukarıdaki gibi bir kullanımda ekrana string olarak basıyor, yani değişken içinde html kodları olsaydı onları yorumlamak yerine direk ekranda basacaktı (Hello <b>World</b> gibi):

```jsx
 <div id="app">
      <p v-text='text'></p>
      <!-- <b>Hello World</b> -->
      <p v-html='text'></p>
      <!--    Hello World     -->
  </div>
<script>
    new Vue({
        el:'#app',
        data:{
            text:'<b>Hello World</b>'
        }
    });
</script>
```

Ekrana bu şekilde değişken basabiliriz peki ya attribute olarak kullanmak istersek mesela href='{{ degisken }}' olarak mı tanımlayacağız? hemen bakalım:

```jsx
<div id="app">
  <! a href='{{ link }}'>Hatalı kullanım</a>
	<a v-bind:href='link'>Doğru kullanım</a>	 
	<! yada kısaca şu şekilde de tanımlanabilir
	<! a :href='link' >Kısa Kullanım</a>
</div>
<script>
    new Vue({
        el:'#app',
        data:{
            link:'https://google.com'
        }
    });
</script>
```

bu sade href işlemi için değiş tüm attribute'ler için geçerlidir. Hatta elemanın sahip olmadığı bir attr bile tanımlayabilirsiniz.

**Event Binding,**

click, ready, focus gibi eventlerin çalıştırdığı fonksiyonlar vardır. Bunlar dinamik yapı oluşturmada yardımcı olurlar. Nasıl tanımlanır hemen bakalım.

```jsx
<div id="app">
	<button v-on:click='tikla()'> Arttır </button>	 <! click yazdığımız yerde hangi event lazımsa onu yazabiliriz
	<p v-text='sayi'><p>  <! sayı değişkenimizi ekrana basıyoruz
</div>
<script>
    new Vue({
        el:'#app',
        data:{
						sayi:0
        },
				methods:{//method tanımlamalarımızı bu parantezler içinde yapacağız
					tikla(){// tikla adında bir fonksiyon oluşturuyoruz ve butonumuzun click eventinde çağırıyoruz
						this.sayi++// her çağrıda sayının bir artmasını istiyoruz
					}
				}
    });
</script>
```

Ekrana Değişken yazmayı ve eventleri öğrendik o zaman şöyle bir örnek yapalım.

Mesela sadece toplama işlemi yapan bir hesap makinesi

Bir inputumuz olsun buraya girilen değer enter tuşuna basıldığında hemen altındaki değişkene yazdırılsın

```jsx
<div id="app">
    <input type="text" v-on:keypress='keyp($event)'>
    <! $event vue ile gelen bir değişkendir. 
    <! Bize gerçekleşen event işleminin ayrıntılarını verir
		<! v-on yerine kısaca @ kullanabilirsiniz
		<! input type="text" @keypress='keyp($event)'>
    <p> {{ sayi }} </p>
</div>
<script>
    new Vue({
        el:'#app',
        data:{
            sayi:0
        },
        methods:{
            keyp(e){
                console.log(e) 
                //bize tam olarak neler geldiğini görmek isterseniz
                if(e.keyCode==13){//enter tuşunun key codu
                  this.sayi += parseInt(e.target.value)
                  //e.target bizim inputumuzun ayrıntılarını verir
                  //burada yaptığımız sayimiza inputtaki değeri eklemek
                }

            }
        }
    });
</script>
```

Eventleri tek tek araştırmak enter neydi diye  uzun uzun kodlar yazmak yerine şu şekilde bir kolaylığımız da var

**Event modifiers**

```jsx
<div id="app">
    <input type="text" @keypress.enter='keyp($event)'>
    <p> {{ sayi }} </p>
</div>
<script>
    new Vue({
        el:'#app',
        data:{
            sayi:0
        },
        methods:{
            keyp(e){//artık sadece enter tuşuna basıldığında çalışacak
                  this.sayi += parseInt(e.target.value)
                }
            }
        }
    });
</script>
```

Şimdi şöyle bir senaryo kuralım bir input bir de paragraf elemanlarımız olsun ben inputa girdiğim değerin aynı anda paragrafta da değişmesini istiyorum. Bunun için kullanacağımız şey:

**Two way data binding (iki yönlü veri doğrulama)**

```jsx
<div id="app">
    <input type="text" v-model='sayi'><!model iki yönlü olarak bağlamaya imkan sağlıyor
		<! artık veri değiştirmek için fonksiyon kullanmamıza gerek kalmıyor
    <p> {{ sayi }} </p>
</div>
<script>
    new Vue({
        el:'#app',
        data:{
            sayi:0
        },
       
    });
</script>
```

**Methods  -  Computed  -  Watch**

1.Methods

Methodların temel mantığı çalıştığında bizim el olarak tanımlanan app bloğunun içini tazelemektir.

Bunun mantığını anlamak için şunu yapabilirsiniz bizim değişken yazdırmak için kullandığımız süslü parantexler içinde tek satır javascript kodu yazabilirsiniz: {{ alert() }}, ve bununla hiç alakası olmayan bir method çalıştırın mesela butona tıklandığında sayıyı 1 arttıran method yukarıda yapmıştık. Siz butona bastığınızda aslında hiç alakası olmamasına rağmen alert verdiğini göreceksiniz. İyi de bemn onu çağırmadım orda alert yerine daha kapsamlı ve server yoracak bir fonksiyon olsaydı ne olacaktı. Bunun için kullanabileceğimiz 2 şey var.

2.Computed

Yine methods gibi tanımlama yapıp içine fonksiyonlarımızı yazıyoruz. Bunun çalışma mantığı ise şöyledir. Bir fonksiyon oluşturduk içinde ise değişkenlerimiz var. Kendince bir kontrol yapıyor benim değişkenlerimde bir değişiklik oldu mu yada beni ilgilendiren bir durum var mı. Eğer yoksa çalışmamayı tercih ediyor. Yani kısaca computed bir kere çalışacak ağır fonksiyonlarımız için kullanabiliriz.3.

3.Watch

Burda ise otomatik kontrol değil birebir kontrol yapıyoruz. Değişken değişti ise direkt olarak değişkeni izlediğimiz fonksiyonlar yazıyoruz. Bu fonksiyonlar bize eski ve yeni değerlerini kontrol edebilmemizi de sağlarlar. 

Şimdi bir örnekle daha iyi anlayalım

```jsx

    <div id="app">
        <input type="text" v-model="age">
        <p> JavaScript : {{ age >= 18 ? 'Reşittir' : 'Değildir...' }} </p><! App içerisinde herhangi bir tetikleme ile çalışır
        <p> Method : {{ ageWriter() }} </p><! App içerisinde herhangi bir tetikleme ile çalışır
        <p> Computed : {{ ageWriterComp }}</p><! Onu ilgilendiren bir durum yoksa çalışmaz
        <hr>
        <p> {{ sayi }} </p>
        <button @click="sayi++">Sayı Arttır..</button>
    </div>

    <script>
      new Vue({
        el: "#app",
        data : {
          age : 0,
          sayi : 0
        },
        methods : {//click tuşuna bastığımızda yada input içinde değişiklik yaptığımızda çalışır
          ageWriter(){
            console.log("Method Çalıştı...")
            return this.age >= 18 ? 'Reşittir' : 'Değildir...'
          }
        },
        computed : {
          ageWriterComp(){
					//click tuşuna bastığımızda yada input içinde değişiklik yaptığımızda age değişkeninde bir değişiklik var mı 
					//age değişkeninde bir değişim varsa çalışır yoksa çalışmaz
            console.log("Computed Çalıştı...")
            return this.age >= 18 ? 'Reşittir' : 'Değildir...'
          }
        },
        watch : {
          age(newValue,oldValue){//fonksiyon ismi hangi değişkeni takip edeceğimizi belirler
            console.log("New : " + newValue)//Yeni değeri
            console.log("Old : " + oldValue)//Eski değeri
          } 
        }
      });
    </script>
```

Vue.js ile style dosyası üzerinde nasıl oynayabiliriz. 

Mesela butona tıkladığımızda divin arka planını değiştirsek.

```jsx
<!DOCTYPE html>
<html lang="tr">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Vue.js </title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
      .box {
        width: 200px;
        height: 200px;
        border: 1px solid #666;
        background-color: #fbbd08;
        margin-bottom: 10px;
      }

      .blue {
        background-color: blue;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <div class="box" :class="className"></div><! sınıflarımızı hem dinamik hem de statik olarak ekleyebiliriz
      <div class="box" :style="{'background-color' : 'black'}"></div>// stillerimi obje olarak tanımlayabiliriz
      <button @click="showBlue = !showBlue">Ekle/Kaldır</button>//amaç butona tıklandığında divin arkaplanını değiştirmek
    </div>

    <script>
      new Vue({
        el: "#app",
        data: {
          showBlue: false
        },
        computed: {
          className() {
            return {
                'blue' : this.showBlue
            };
          }
        }
      });
    </script>
  </body>
</html>
```

**Koşullar (v-if)**

Koşullar diğer programlama dillerindeki aynı mantıkla çalışıyor. Yani şöyle bir örnek yapabiliriz tıklandığında paragrafları gösterip gizle ve bunu koşulla kontrol et.

```jsx
<div id="app">
    <button @click="showBox = !showBox">Göster/Gizle</button> <! showBox değerini tam tersi yap 
    <p v-if="showBox">Bu sayfa açıldığında gösterilecek..</p> <! showBox true ise içerik gözüksün
    <p v-else-if="!showBox">Bu sayfa acildiginda gösterilmeyecek.. Buton yardimiyla gözükecek...</p><! showBox false ise içerik gözüksün
    <p v-else>Aksi halde gözükecek blüm...</p><! showBox çok daha farklı bir değer olarak gelirse gözüksün
</div>

<script>
    
    new Vue({ 
        el : "#app",
        data : {
          showBox : false
        }
    });

</script>
```

**Objeler  -  Diziler  -  Dögüler**

Yine aşağı yukarı aynı mantıkta olan dögüleri hemen kodlar üzerinde inceleyelim

```jsx
	
<div id="app">
  <ul>
    <li v-for="item in list">{{ item }}</li><! kısaca foreach olarak görebilirsiniz list dizisi içinden itemleri yazdır
		<li v-for="item in 10">{{ item }}</li><! kısaca for döngüsü 10'a kadar dönmesini sağlayabilirsiniz
  </ul>
  <hr />
  <div v-for="(value, key) in personel"><! tek değerli obje kullanımı
    <strong>{{ key }} </strong> <span>{{ value }}</span>
  </div><! keyleri ve valueleri ile beraber her şeyi bu div içine yaz
</div>
<script>
  new Vue({
    el: "#app",
    data: {
      list: ["Elma", "Armut", "Kiraz"],//dizimizi tanımlıyoruz
      personel: {//objemizi tanımlıyoruz
        name: "Erdoğan",
        lastname: "Yeşil",
        email: "erdogan@yesil.com"
      }
    }
  });
</script>
```

/
