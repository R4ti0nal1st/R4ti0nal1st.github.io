---
layout: post
title: 'C++ без Visual Studio'
date: 2015-09-16 00:40
comments: true
categories: 
---
Я решаю задчки по программированию на стареньком ноутбуке, потому что на моей основной машине стоит Windows 10 и я вынужден держать на ней VS 2012, которая скорее не понимает современного C++, нежели понимает. Обновляться до VS 2015 я не хочу, потому что боюсь, что поломается старый код.

Как раз на днях у меня был разговор, про изучение нового языка программирования. Я говорил, что прежде всего надо обзавестить хорошим источником информации и настроить удобное окружение. Потому мотивацию не сохранить, если не получается быстро начать получать результаты.

Вспомнив об этом разговоре, я решил настроить себе удобное окружение для решения олимпиадных задачек под Windows.

Оно состоит из:
  - GCC 5.2.0
  - Clang 3.7.0
  - CMake
  - Sublime Text 3
    - SublimeLinter3
    - SublimmeLinter-Clang
  - Eclipse (если надо отлаживать)
 
 В итоге получится примерно следующее:
 
 ![sublime-clang.png](http://user-image.logdown.io/user/14217/blog/13433/post/300509/krtDQRiKRQSuMBHxQS0y_sublime-clang.png)
 
 ![cmake-build.png](http://user-image.logdown.io/user/14217/blog/13433/post/300509/ofhmFD2dS2G1VNn2rZY8_cmake-build.png)
 
По моему, совсем не плохо.

Как всё это ставить.

1. Ставим MSYS2 c http://msys2.github.io/ и обновляем пакеты как описано в инструкции.
2. Ставим Make и GCC.
```
pacman -S mingw-w64-x86_64-gcc
pacman -S mingw-w64-x86_64-make
```
3. Создаём переменную окружения `MINGW_HOME=<MSYS2>\mingw64`.
4. Добавляем `%MINGW_HOME%\bin` в `PATH`.
5. Ставим Clang с http://clang.llvm.org/ и ставим галочку про добавление в `PATH`.
6. Копируем `mingw32-make` в `make`, а `x86_64-w64-mingw32-*` в `mingw32-*`. Первое надо для удобства. Без второно Eclipse не найдёт toolchain. Это описано в [FAQ] (http://wiki.eclipse.org/CDT/User/FAQ) в секции "I installed MinGW toolchain on my PC but Eclipse won't find it."
7. Ставим CMake с http://www.cmake.org/.
8. Ставим Sublime Text 3 с http://www.sublimetext.com/.
9. Устанавливаем Package Control для Sublime Text 3, как описано в https://packagecontrol.io/installation.
10. Ставим следующие пакеты для Sublime Text 3:
  - https://packagecontrol.io/packages/SublimeLinter
  - https://packagecontrol.io/packages/SublimeLinter-contrib-clang
11. Для Clang плагина может потребоваться прописать дополнительные директории с include. У меня его настройки выглядят так:

    ```
    "linters": {
        "clang": {
            "@disable": false,
            "args": [],
            "excludes": [],
            "extra_flags": "-std=c++14",
            "include_dirs": [
                "${project_folder}/build/gtest/src/gtest/include",
                "${project_folder}/build/gmock/src/gmock/include"
            ]
        }
    },
    ```

Соответсвенно, я создаю файл проекта, чтобы всё это отработало.
12. Ставим Eclipse с https://eclipse.org.
13. Чтобы всё нормально собиралось и работало, я добавил в проект опции `-static-libgcc -static-libstdc++`, как описано [тут] (https://orfe.princeton.edu/help/article-296) и [тут] (http://stackoverflow.com/questions/18138635/mingw-exe-requires-a-few-gcc-dlls-regardless-of-the-code).

Некуда приткнуть, но в одном из моих любимых блогов тоже есть [инструкция] (http://preshing.com/20141108/how-to-install-the-latest-gcc-on-windows/) по установке свежего GCC под Windows.
