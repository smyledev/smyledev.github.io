# Синтаксис и особенности Java 

```java
1. Многопоточность.

// Создание потока с выводом 100 первых чисел...
Thread thread1 = new Thread(() -> {
  for (int i = 0; i < 100; i++) {
    System.out.println("Thread 1: " + i);
  }
});



//Запуск потока...
thread1.start();

//Ожидание завершения работы потока...
thread1.join();



// Проверка флага прерывания потока...
Thread.interrupted();

// Установление прерывающего флага...
thread.interrupt();



// Создание псевдослучайного числа...
Random random = new Random(1);

// Генерация 100 случайных чисел...
List <Integer> list1 = random.ints(100).boxed().collect(Collectors.toList());



// Поиск максимального числа из списка...
Integer max = list1.stream().reduce((a, b) -> Math.max(a, b)).get();



// Мьютекс 1 способ(синхронизация потоков, операции добавления)...
synchronized (maxs) {
  maxs.add(max);
}

// Мьютекс 2 способ(более функциональный, наглядный в виде объекта)...
Lock lock = new ReentrantLock();

lock.lock();

try {
  maxs.add(max);
}
finally {
  lock.unlock(); 
}



// Создание собственной потокобезопасной коллекции...
List<Integer> maxs = Collections.synchronizedList(new ArrayList<>());



// Создание пула из 10 потоков...
ExecutorService pool = Executors.newFixedThreadPool(10);

// Запуск потока для подсчета максимального значения...
Future<Integer> f1 = pool.submit(() -> list1.stream().reduce(Math::max).get());

Future<Integer> f1 = pool.submit(() -> list1.stream().reduce(Math::max).get());

// Ожидание и получение значения...
Integer max1 = f1.get();

Integer max2 = f2.get();



// Создание ассинхронной работы с возвратом значения при выполнении(callback),
// sypplyAsync позволяет обрабатывать исключения, если действие не выполненно...
CompletableFuture<Integer> f1 = CompletableFuture.supplyAsync(() ->
        list1.stream().reduce(Math::max).get(), pool);

CompletableFuture<Integer> f2 = CompletableFuture.supplyAsync(() ->
        list2.stream().reduce(Math::max).get(), pool);

// Получение списка значений полученных из 2 потоков...
CompletableFuture<List<Integer>> result = f1.thenCompose((m1) -> f2.thenApply(m2 -> List.of(m1, m2)));




2. Потоки.


// Созание потока ввода, передаем файл для чтения, считываем блоками байт.
// 1 вариант...
public class Main {

  public static void main(String[] args) throws IOException {

    InputStream inputStream = new FileInputStream("file.bin");

    try {
      byte[] bytes = new byte[1024];

      while (true){
        int read = inputStream.read(bytes);
        if (read == -1) break;
        System.out.println(Arrays.copyOf(bytes, read));
      }
    } finally {
      inputStream.close();
    }

  }

}


// 2 вариант...
public class Main {

  public static void main(String[] args) throws IOException {

    try (InputStream inputStream = new FileInputStream("file.bin")){
      byte[] bytes = new byte[1024];

      while (true){
        int read = inputStream.read(bytes);
        if (read == -1) break;
        System.out.println(Arrays.copyOf(bytes, read));
      }
    }
  }

}



// Реализация декоратора потока вывода...
try (OutputStream outputStream = new BufferedOutputStream(
        new FileOutputStream("file.bin"))){

  outputStream.write(new byte[]{1, 4 , 5});
  outputStream.flush();

}


3. Дополнительные Возможности


// Запись в архив...
public class Main {

  public static void main(String[] args) throws IOException {

    try (PrintWriter printWriter = new PrintWriter(new GZIPOutputStream(new FileOutputStream("file.gz")))){

      for (int i = 0; i < 50; i++){
        printWriter.println("Hello world" + i);
      }

    }

  }

}


// Класс для создания архива, содержащего аудиофайл...

package ru.samoilov;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.zip.ZipEntry;
import java.util.zip.ZipOutputStream;


class Zip{

  public static void main(String[] args) {

    // Путь исходного аудиофайла...
    String filePath = "MusicFile.wav";

    // Путь архива, в который запишем аудиофайл...
    String archivePath = "ZipSound/ZipFile.zip";

    // Создаем поток вывода для архива...
    // Создаем поток ввода для считывания аудиофайла...
    try (ZipOutputStream zipOutputStream = new ZipOutputStream(new FileOutputStream(archivePath));
         FileInputStream fileInputStream= new FileInputStream("Sound/" + filePath);){

      // Добавляем в архив файл...
      ZipEntry zipEntry = new ZipEntry(filePath);
      zipOutputStream.putNextEntry(zipEntry);

      // Записываем звуковой файл в архив...
      byte[] buffer = new byte[fileInputStream.available()];
      fileInputStream.read(buffer);
      zipOutputStream.write(buffer);

      zipOutputStream.closeEntry();
    } catch (IOException e){
      e.printStackTrace();
    }

    System.out.println("Music file was added to archive");

  }
}


// Воспроизведение звукового файла(.wav)...

import javax.sound.sampled.*;
import java.io.File;

public class Main {

  public static void main(String[] args) {

    playSound("Sounds/MusicFile.wav");

  }

  public static synchronized void playSound(final String file_name) {

    new Thread(new Runnable() {
      public void run() {
        try {
          File file = new File(file_name);
          AudioInputStream inputStream = AudioSystem.getAudioInputStream(file);

          // Смотрим какого формата звуковой файл...
          // AudioFormat format = audioInputStream.getFormat();
          // System.out.println(format);

          Clip clip = AudioSystem.getClip();
          clip.open(inputStream);
          clip.start();
          Thread.sleep(4000);
        } catch (Exception e) {
          e.printStackTrace();
        }
      }
    }).start();

  }

}
```
