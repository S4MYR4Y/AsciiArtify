Демонстрація роботи продукту на розгорнутій вами інфраструктурі (Файл doc/MVP.md у форматі Markdown, гілка main)

1. Створення нового застосцнку

<img width="1004" height="538" alt="Image" src="https://github.com/user-attachments/assets/3dfb88b7-0bd9-47d8-a014-144c1e5f0a1c" />

<img width="1004" height="392" alt="Image" src="https://github.com/user-attachments/assets/2b342ff7-ef27-4d41-bbac-9f8ce3ba8496" />

  У графу repository URL вставимо посилання на гіт-хаб, що має маніфести для розгортання застосунку

 https://github.com/den-vasyliev/go-demo-app

<img width="1004" height="350" alt="Image" src="https://github.com/user-attachments/assets/ee90a03d-d929-4e8e-8904-4d8f11d01d3c" />

  Нажміть кнопку створити

<img width="611" height="622" alt="Image" src="https://github.com/user-attachments/assets/42ee38ae-638c-425a-adfa-8919b06a4662" />


2. Синхронізація додатку

  У головному вікні натиснемо кнопку "Sync"

<img width="738" height="741" alt="Image" src="https://github.com/user-attachments/assets/940e80cf-d119-4f16-b115-786dcee98b9d" />

  Треба трохи почекати поки усі файли синхронізуються

  Після синхронізації побачимо, що застосунок має бажаний стан:

<img width="1004" height="159" alt="Image" src="https://github.com/user-attachments/assets/30d0eb9e-5b55-4b3d-ae26-26910ef0f623" />

3. Перевірка роботи застосунку

   Переадресуємо порти наступною командою:

```bash
k port-forward -n argocd svc/ambassador 8081:80
```
  Перевіримо, що port-forward працює:

```bash
k port-forward -n argocd svc/ambassador 8088:80    
Forwarding from 127.0.0.1:8088 -> 80
Forwarding from [::1]:8088 -> 80

curl localhost:8088
k8sdiy-api:599e1af%  
```
  Після цього завантажуємо довільне зображення
```bash
    wget -O enli.png https://cdn2.steamgriddb.com/logo_thumb/cab928fef1ab1896dd4a6279b1d70f2f.png
--2025-08-14 18:21:41--  https://cdn2.steamgriddb.com/logo_thumb/cab928fef1ab1896dd4a6279b1d70f2f.png
Resolving cdn2.steamgriddb.com (cdn2.steamgriddb.com)... 104.21.112.1, 104.21.96.1, 104.21.32.1, ...
Connecting to cdn2.steamgriddb.com (cdn2.steamgriddb.com)|104.21.112.1|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 121720 (119K) [image/png]
Saving to: ‘enli.png’

enli.png                                             100%[====================================================================================================================>] 118.87K  --.-KB/s    in 0.05s   

2025-08-14 18:21:42 (2.54 MB/s) - ‘enli.png’ saved [121720/121720]
```
  І після цього виводимо зображення у терміналі
```bash
    curl -F 'image=@enli.png' localhost:8088/img/                                                
                                                                                                    
   .                                             1t                                             .   
   :;;,.                                        ,@@:                                        .,;;:   
     .,;;;,                                     G  0                                     ,:i;:.     
        .,;ii:.                                1@CC@t                                .,;i;,.        
            ,it1;,                            ,8 ;,8@:                            ,;ii;,            
               .;it1i,                        L@L  t@G                        ,i1t1:.               
                   :1LCt;iff                 i@8.  .0@i                 fL;;fCt1:.                  
                      ,;ttLCti.             ,8@1    ;@@,             .;fLLtti,                      
                          .;iL0C1;          L@G      C C          :tGGLt;,                          
                              :f0@0Lt,     i@@,      ,@@;     ,fC8@0f:.                             
    ;t:,,,,:,,,:,,::,,,,,,,::,,.,iLG88G1;:,8@f        1@8::;1LG8GLi:,..::,,,,,:,,,,,::,,,:::,:t;    
   ,1t1ii11111ii1t11t1i1111iii111;;;1ttttttt1          1tttt111i;;iii;i1i;i11iii1t1iii111iiitt1i,   
      ifft111tLt  1f,     ;1f1. ifft:     ,1tf1,  ;ffttt:  if11tft11fi,1ff1111tfi ,1ftiii1i,        
      .G@1,,,,1@, ;@@f.    ,@1  .0@1       ;@8.  L@f  .1@, 0L,.L@f.,CG :@@:,,,,C0  ;@8...:t@G:      
       C@;     :. ,80@8i    8;   C@;       ,@0   C@0i.  ,. :   t@1  .: .@8     .:  ,@0     ;@8,     
       C@L1t10t   ,@:;0@C, .@i   C@;       , 0    iC@@0t,      f@t     .@@tt1t8,   ,@0      G 1     
       C@1,:,1;   ,@: .t@@t.8i   C@;       ,@0   .  .;L@@t     f@t     .@8:,::t.   ,@0      G@i     
       L@:     ,i ,@,   ,L@08;   C@;    ;, ,@0  ,@,    , @,    t@1     .@8      ;: ,@0     1@G.     
      ,0@f:;:;i8G ;@1,.   ;G@i .,0@f:;;t@i.i@@:..8G;,,:f@f    ,C@L.   .;@@i:::;t@1.i@@;::iC8f.      
      ;iiiiiii11: iii;.     ;.  ;ii1iiit1.,111i,  ;1111i,     :iii:   ,iiiii11111.,iiiiiii:         
   ,1t1111111ii;;1iiii;;1ttt1tff1i1iitt.         ,          .ffii1itLC11tfti;:;;i11ii1iiiiitttLt,   
    .;,,,,.,.........,iC@@@@@8Gti1;.G@i       ,tG8001.       i@0.;itfC0@@@@@Gi:............,,:i,    
                     1@@@@@C1t1i;. 1@f      :f80f:,tG0L;      t@t  :1;:;L@@@@@;                     
                    f@@@8f;.;f;   :@8.   :fGGf;      :f0Gt,    G@:   :t;.;C8@@8L.                   
                ..;G@@@L.:1;,     0 : .1G0Ci.          .iCGCt, .@0     ,;i;,f@@@0i..                
            ,iL088@@@0;          t@t;C8G1,                .iC0Ci1@L          :L@@@880C1,            
         :tG@@@@  @01           1@@GGt:                      ,tGG@@1           i0@  @@@@Gt:         
      .10@@@ @@ @8t.           ,8Gt:                            :tC0,            18@ @@ @@@0f:      
    ;C8@@ @@@@@@f.             ,.                                  .,             .f@@ @@@ @@@Ci    
   C@@ @@@@@@@C,                                                                    ,L@@@@@@@ @@0.  
   ;C@ @@@@@G;                                                                        :C@@@@@ @G;   
     18@@@0i                                                                            ;G@@@@1     
      .tCi                                                                                ;Cf,      
```                                                                                             
