# Домашнее задание к работе 18
## Условие задачи
Создать структуру для хранения указанной в индивидуальном
варианте записи, организовать в программе ввод 5-10 различных
записей, из полученного массива структур найти и напечатать
информацию согласно индивидуальному варианту:

Вариант 14. 

Запись «Игрок»:

Фамилия

Дата и место рождения

Амплуа

Количество игр

Количество желтых карточек

Вывести сведения о футболистах, старших 20- лет и
получающих не более 1 карточки за 10 игр.

## 1. Реализация программы

#include<locale.h>

#include<stdio.h>

#include <stdlib.h>

#include <string.h>

#include <time.h>


// Структура для хранения даты рождения

typedef struct {

    int day;
    int month;
    int year;
    char place[50];
} BirthDate;

// Структура для хранения информации об игроке

typedef struct {

    char surname[50];
    BirthDate birth;
    char position[30];
    int games;
    int yellow_cards;
} Player;

int calculate_age(BirthDate birth) {

    time_t t = time(NULL);
    struct tm* current_time = localtime(&t);

    int current_year = current_time->tm_year + 1900;
    int current_month = current_time->tm_mon + 1;
    int current_day = current_time->tm_mday;

    int age = current_year - birth.year;

    if (current_month < birth.month ||
        (current_month == birth.month && current_day < birth.day)) {
        age--;
    }

    return age;
}

int check_cards_condition(Player player) {

    if (player.games == 0) return 0; 

    
    int blocks_of_10_games = player.games / 10;
    if (player.games % 10 != 0) {
        blocks_of_10_games++; 
    }

    return player.yellow_cards <= blocks_of_10_games;
}

void input_player(Player* player) {

    printf("\nВведите фамилию игрока: ");
    scanf("%s", player->surname);

    printf("Введите дату рождения (день месяц год): ");
    scanf("%d %d %d", &player->birth.day, &player->birth.month, &player->birth.year);

    printf("Введите место рождения: ");
    scanf("%s", player->birth.place);

    printf("Введите амплуа игрока: ");
    scanf("%s", player->position);

    printf("Введите количество игр: ");
    scanf("%d", &player->games);

    printf("Введите количество желтых карточек: ");
    scanf("%d", &player->yellow_cards);
}

void print_player(Player player) {

    printf("\nФамилия: %s\n", player.surname);
    printf("Дата рождения: %02d.%02d.%d\n", player.birth.day, player.birth.month, player.birth.year);
    printf("Место рождения: %s\n", player.birth.place);
    printf("Амплуа: %s\n", player.position);
    printf("Количество игр: %d\n", player.games);
    printf("Желтых карточек: %d\n", player.yellow_cards);
}

int main() {

    setlocale(LC_CTYPE, "RUS");
    
    int num_players;
    Player players[10];

    printf("Сколько игроков вы хотите ввести (5-10)? ");
    scanf("%d", &num_players);

    if (num_players < 5 || num_players > 10) {
        printf("Ошибка! Введите от 5 до 10 игроков.\n");
        return 1;
    }

    printf("\n=== Ввод информации об игроках ===\n");
    for (int i = 0; i < num_players; i++) {
        printf("\nИгрок #%d:\n", i + 1);
        input_player(&players[i]);
    }

    printf("\n=== Все введенные игроки ===\n");
    for (int i = 0; i < num_players; i++) {
        print_player(players[i]);
    }

    printf("\n=== Игроки, соответствующие условиям ===\n");
    printf("(старше 20 лет и не более 1 желтой карточки за 10 игр)\n");

    int found = 0;
    for (int i = 0; i < num_players; i++) {
        int age = calculate_age(players[i].birth);
        int cards_ok = check_cards_condition(players[i]);

        if (age > 20 && cards_ok) {
            printf("\n--- Игрок #%d ---", i + 1);
            print_player(players[i]);
            printf("Возраст: %d лет\n", age);
            found = 1;
        }
    }

    if (!found) {
        printf("\nИгроки, соответствующие условиям, не найдены.\n");
    }

    return 0;
}

## 2. Результаты работы программы

Сколько игроков вы хотите ввести (5-10)? 5

=== Ввод информации об игроках ===

Игрок #1:

Введите фамилию игрока: A

Введите дату рождения (день месяц год): 16 12 2000

Введите место рождения: A

Введите амплуа игрока: CP

Введите количество игр: 24

Введите количество желтых карточек: 2

Игрок #2:

Введите фамилию игрока: B

Введите дату рождения (день месяц год): 04 04 2005

Введите место рождения: B

Введите амплуа игрока: CZ

Введите количество игр: 3

Введите количество желтых карточек: 3

Игрок #3:

Введите фамилию игрока: C

Введите дату рождения (день месяц год): 01 02 2003

Введите место рождения: C

Введите амплуа игрока: V

Введите количество игр: 15

Введите количество желтых карточек: 2

Игрок #4:

Введите фамилию игрока: D

Введите дату рождения (день месяц год): 25 12 2004

Введите место рождения: D

Введите амплуа игрока: Z

Введите количество игр: 12

Введите количество желтых карточек: 4

Игрок #5:

Введите фамилию игрока: E

Введите дату рождения (день месяц год): 15 11 2008

Введите место рождения: E

Введите амплуа игрока: N

Введите количество игр: 5

Введите количество желтых карточек: 0

=== Все введенные игроки ===

Фамилия: A

Дата рождения: 16.12.2000

Место рождения: A

Амплуа: CP

Количество игр: 24

Желтых карточек: 2

Фамилия: B

Дата рождения: 04.04.2005

Место рождения: B

Амплуа: CZ

Количество игр: 3

Желтых карточек: 3

Фамилия: C

Дата рождения: 01.02.2003

Место рождения: C

Амплуа: V

Количество игр: 15

Желтых карточек: 2

Фамилия: D

Дата рождения: 25.12.2004

Место рождения: D

Амплуа: Z

Количество игр: 12

Желтых карточек: 4

Фамилия: E

Дата рождения: 15.11.2008

Место рождения: E

Амплуа: N

Количество игр: 5

Желтых карточек: 0

=== Игроки, соответствующие условиям ===

(старше 20 лет и не более 1 желтой карточки за 10 игр)

--- Игрок #1 ---

Фамилия: A

Дата рождения: 16.12.2000

Место рождения: A

Амплуа: CP

Количество игр: 24

Желтых карточек: 2

Возраст: 24 лет


--- Игрок #3 ---

Фамилия: C

Дата рождения: 01.02.2003

Место рождения: C

Амплуа: V

Количество игр: 15

Желтых карточек: 2

Возраст: 22 лет

## 3. Информация о разработчике

Лычагин Антон бИЦ-252
