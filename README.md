# SO_3

Zadanie polega na dodaniu wywołania systemowego PM_NEGATEEXIT oraz funkcji bibliotecznej int negateexit(int negate). Funkcja powinna być zadeklarowana w pliku unistd.h.
Negacja kodu powrotu procesu

W MINIX-ie proces kończy działanie, wywołując _exit(status), gdzie status to kod powrotu procesu. Rodzic może odczytać kod powrotu swojego potomka, korzystając np. z wait. Powłoka umieszcza kod powrotu ostatnio zakończonego procesu w zmiennej $?. Chcemy umożliwić procesowi wpływanie na wartość kodu powrotu swojego i swoich nowo tworzonych dzieci.

Nowa funkcja int negateexit(int negate), gdy zostanie wywołana z parametrem różnym od zera, powoduje, że gdy proces wywołujący tę funkcję zakończy działanie z kodem zero, rodzic odczyta kod powrotu równy jeden, a gdy zakończy działanie z kodem różnym od zera – rodzic odczyta zero. Wywołanie tej funkcji z parametrem równym zeru przywraca standardową obsługę kodów powrotu.

Wartość zwracana przez tę funkcję to informacja o zachowaniu procesu przed wywołaniem funkcji: 0 oznacza, że kody powrotu nie były zmieniane, a 1 – że były negowane. Jeśli wystąpi jakiś błąd, należy zwrócić -1 i ustawić errno na odpowiednią wartość.

Nowo tworzony proces ma dziedziczyć aktualne zachowanie rodzica, natomiast przyszłe zmiany zachowania rodzica (wynikające z kolejnych wywołań negateexit()) nie mają wpływu na potomka.

Jeżeli proces kończy działanie w inny sposób, niż używając systemowego wywołania PM_EXIT (używanego przez funkcję _exit()), np. na skutek sygnału, to jego kod powrotu nie powinien zostać zmieniony.

Na początku działania systemu, w szczególności dla procesu init, kody powrotu nie mają być negowane.

Działanie funkcji negateexit() powinno polegać na użyciu nowego wywołania systemowego PM_NEGATEEXIT, które należy dodać do serwera PM. Do przekazania parametru należy zdefiniować własny typ komunikatu.
