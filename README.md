# fuzzing_zsh
# Лабораторная работа № 1

Считаем хэш каждого файла

```
find zsh/ -type f -exec sha256sum {} \; > hashes.txt
```

![изображение](https://github.com/user-attachments/assets/3d269b2c-cdf4-45ba-a13c-84dcd6e4d46a)

 Получим хэш последнего коммита 
 
 ![изображение](https://github.com/user-attachments/assets/c754d2de-f6ba-4c49-beb7-5d428e81fbb6)

 # Лабораторная работа № 2
 
  Скачиваем проект фаззер

```
git clone https://github.com/AFLplusplus/AFLplusplus.git
cd AFLplusplus/ 
```

 ![изображение](https://github.com/user-attachments/assets/eb5b7274-917a-4801-8b0d-56786d9a4861)
 
```
ls
make
```

![изображение](https://github.com/user-attachments/assets/20ed1172-f7a8-484b-b2a5-2a05168be6de)

```
git clone https://github.com/zsh-users/zsh.git
ls
cd zsh/
autoconf
autoheader
CC=/home/masha/fuzzing/AFLplusplus/afl-gcc CXX=/home/masha/fuzzing/AFLplusplus/afl-gcc++ ./configure
make
make install
```

![изображение](https://github.com/user-attachments/assets/a4db7b75-cb68-4e8e-b476-bf15c57ca3cc)

![изображение](https://github.com/user-attachments/assets/3444f7d8-4f1b-4889-b491-653e0d28055b)

![изображение](https://github.com/user-attachments/assets/5fce00db-dc83-4759-905e-5155404d4898)

```
mkdir input
cd input/
echo "Hello, test" > test1.txt
echo "Hello, testing 2" > test2.txt
```
![изображение](https://github.com/user-attachments/assets/fef9258d-d8c5-482b-be46-78425a660968)

Запустим фаззинг 

```
/home/masha/fuzzing/AFLplusplus/afl-fuzz -i /home/masha/fuzzing/input/ -o /home/masha/fuzzing/out/ -- ./zsh @@
```

![изображение](https://github.com/user-attachments/assets/6cc2daf8-0082-4959-8e6d-f8c3ed70986d)

![изображение](https://github.com/user-attachments/assets/eee1339a-d9a1-4c1d-a790-47b1c8700aa9)

```
sudo apt install gnuplot
afl-plot ./out/default/ plot_data
```

![изображение](https://github.com/user-attachments/assets/41ed5170-46c8-4687-9d55-5dcebb0a4d1c)

![изображение](https://github.com/user-attachments/assets/5c87ba93-66fe-4b02-9706-c77efa8ba0b3)

![изображение](https://github.com/user-attachments/assets/6eb4c283-a0fe-40b6-b4af-575c6746228f)

![изображение](https://github.com/user-attachments/assets/803ca470-b5ab-4625-a5a4-f2426fbf6281)

 # Лабораторная работа № 3

Очистим zsh/ и zsh/Src/ :

```
make clean

```

Cоберем компиляторы для покртыия

``` 
apt install lcov
CC="gcc --coverage" CXX="g++ --coverage" ./configure 
make
```
Запустим

```
for file in /home/masha/fuzzing/out/default/queue/*; do timeout 5 ./zsh "$file" || echo "Skipping $file"; done
lcov -o cov.info -c -d .
```

![изображение](https://github.com/user-attachments/assets/f7e40cb8-02e7-4a92-835c-c9998603f0c0)

Соберем в читабельный вид

```
genhtml -o /home/masha/fuzzing/cov_data cov.info
```

![изображение](https://github.com/user-attachments/assets/7a860c39-7b48-4b5e-b02e-1a419f76cc51)

![изображение](https://github.com/user-attachments/assets/145b6802-3cba-466f-bf9c-8c0f677e4046)







 

