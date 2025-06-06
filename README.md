Завдання 1 - Таймер зворотного відліку

Напиши скрипт таймера, який здійснює зворотний відлік до певної дати. Такий
таймер може використовуватися у блогах, інтернет-магазинах, сторінках реєстрації
подій, під час технічного обслуговування тощо. Подивися демовідео роботи
таймера.

Бібліотека flatpickr Використовуй бібліотеку flatpickr для того, щоб дозволити
користувачеві кросбраузерно вибрати кінцеву дату і час в одному елементі
інтерфейсу. Для того щоб підключити CSS код бібліотеки в проєкт, необхідно
додати ще один імпорт, крім того, що описаний в документації.

// Описаний в документації import flatpickr from "flatpickr"; // Додатковий
імпорт стилів import "flatpickr/dist/flatpickr.min.css";

Другим аргументом функції flatpickr(selector, options) можна передати
необов'язковий об'єкт параметрів. Ми підготували для тебе об'єкт, який потрібен
для виконання завдання. Розберися, за що відповідає кожна властивість у
документації «Options» і використовуй його у своєму коді.

const options = { enableTime: true, time_24hr: true, defaultDate: new Date(),
minuteIncrement: 1, onClose(selectedDates) { console.log(selectedDates[0]); },
};

Вибір дати Метод onClose() з об'єкта параметрів викликається щоразу під час
закриття елемента інтерфейсу, який створює flatpickr. Саме в ньому варто
обробляти дату, обрану користувачем. Параметр selectedDates — це масив обраних
дат, тому ми беремо перший елемент selectedDates[0].

Тобі ця обрана дата буде потрібна в коді і поза межами цього методу onClose().
Тому оголоси поза межами методу let змінну, наприклад, userSelectedDate, і після
валідації її в методі onClose() на минуле/майбутнє запиши обрану дату в цю let
змінну.

Якщо користувач вибрав дату в минулому, покажи window.alert() з текстом "Please
choose a date in the future" і зроби кнопку «Start» не активною. Якщо користувач
вибрав валідну дату (в майбутньому), кнопка «Start» стає активною. Кнопка
«Start» повинна бути неактивною доти, доки користувач не вибрав дату в
майбутньому. Зверни увагу, що при обранні валідної дати, не запуску таймера і
обранні потім невалідної дати, кнопка після розблокування має знову стати
неактивною. Натисканням на кнопку «Start» починається зворотний відлік часу до
обраної дати з моменту натискання.

Відлік часу

Натисканням на кнопку «Start» скрипт повинен обчислювати раз на секунду, скільки
часу залишилось до вказаної дати, і оновлювати інтерфейс таймера, показуючи
чотири цифри: дні, години, хвилини і секунди у форматі xx:xx:xx:xx.

Кількість днів може складатися з більше, ніж двох цифр. Таймер повинен
зупинятися, коли дійшов до кінцевої дати, тобто залишок часу дорівнює нулю
00:00:00:00.

Після запуску таймера натисканням кнопки Старт кнопка Старт і інпут стають
неактивним, щоб користувач не міг обрати нову дату, поки йде відлік часу. Після
зупинки таймера інпут стає активним, щоб користувач міг обрати наступну дату.
Кнопка залишається не активною.

Для підрахунку значень використовуй готову функцію convertMs, де ms — різниця
між кінцевою і поточною датою в мілісекундах.

function convertMs(ms) { // Number of milliseconds per unit of time const second
= 1000; const minute = second _ 60; const hour = minute _ 60; const day =
hour \* 24;

// Remaining days const days = Math.floor(ms / day); // Remaining hours const
hours = Math.floor((ms % day) / hour); // Remaining minutes const minutes =
Math.floor(((ms % day) % hour) / minute); // Remaining seconds const seconds =
Math.floor((((ms % day) % hour) % minute) / second);

return { days, hours, minutes, seconds }; }

console.log(convertMs(2000)); // {days: 0, hours: 0, minutes: 0, seconds: 2}
console.log(convertMs(140000)); // {days: 0, hours: 0, minutes: 2, seconds: 20}
console.log(convertMs(24140000)); // {days: 0, hours: 6 minutes: 42, seconds:
20}

Форматування часу Функція convertMs() повертає об'єкт з розрахованим часом, що
залишився до кінцевої дати. Зверни увагу, що вона не форматує результат. Тобто
якщо залишилося 4 хвилини або будь-якої іншої складової часу, то функція поверне
4, а не 04. В інтерфейсі таймера необхідно додавати 0, якщо в числі менше двох
символів. Напиши функцію, наприклад addLeadingZero(value), яка використовує
метод рядка padStart() і перед відмальовуванням інтерфейсу форматує значення.

Бібліотека повідомлень

Для відображення повідомлень користувачеві, замість window.alert(), використовуй
бібліотеку iziToast. Для того щоб підключити CSS код бібліотеки в проєкт,
необхідно додати ще один імпорт, крім того, що описаний у документації.

// Описаний у документації import iziToast from "izitoast"; // Додатковий імпорт
стилів import "izitoast/dist/css/iziToast.min.css";

Завдання 2 - Генератор промісів

Додай в HTML файл розмітку форми. Форма складається з поля вводу для введення
значення затримки в мілісекундах, двох радіокнопок, які визначають те, як
виконається проміс, і кнопки з типом submit, при кліку на яку має створюватися
проміс.

Напиши скрипт, який після сабміту форми створює проміс. В середині колбека цього
промісу через вказану користувачем кількість мілісекунд проміс має виконуватися
(при fulfilled) або відхилятися (при rejected), залежно від обраної опції в
радіокнопках. Значенням промісу, яке передається як аргумент у методи
resolve/reject, має бути значення затримки в мілісекундах.

Створений проміс треба опрацювати у відповідних для вдалого/невдалого виконання
методах.

Якщо проміс виконується вдало, виводь у консоль наступний рядок, де delay — це
значення затримки виклику промісу в мілісекундах.

Якщо проміс буде відхилено, то виводь у консоль наступний рядок, де delay — це
значення затримки промісу в мілісекундах.

Бібліотека повідомлень

Для відображення повідомлень, замість console.log(), використовуй бібліотеку
iziToast. Для того щоб підключити CSS код бібліотеки в проєкт, необхідно додати
ще один імпорт, крім того, що описаний у документації.

// Описаний у документації import iziToast from "izitoast"; // Додатковий імпорт
стилів import "izitoast/dist/css/iziToast.min.css";
