Starter:
Запускает основной класс StartFactory
StartFactory:
1)Инициализирует файл конфигураций получая информацию.
2)Инициализация складов{
  StockAll:
  1)В конструктор передаются размер класса, после чего создаётся массив ExampleAll(о нём чуть дальше), ставится максимум деталей на складе, какой тип деталей на складе, класс для связи с окном(О нём тоже дальше), и переменная отвечающая за наличие логера
  2)Две функции для получения скорости. Получение скорости производителя, использующего этот склад, и дилера, так как у дилера нет отдельного склада
  3)Две функции на проверку наличия экземпляр на складе и на разрешения положить на склад
  4)Функция на получения свободных в данный момент мест на складе
  5)Функции на то, чтобы взять/добавить деталь
  Несколько типов складов из-за прошлых версий программы, в этой ни один из складов не имеет уникальных функций
  ExampleAll:
  Все детали наследуются от общего класса. У каждой детали есть айди, он задаётся в конструкторе. Интерес представляет только  ExampleCar, там переопределены три функции, чтобы добавить к машине все компоненты
}
3)Инициализация поставщиков. Им в конструктор передаётся класс AllCreature, в который передаётся их тип(1-двигатель, 2- корпус, 3 - аксессуар, 10- машина), а так же подномер для поставщиков деталей и аксессуаров, благодаря этому по Id можно будет определить каким из поставщиков была сделана вещь{
  AllCreature:
  В этом классе хранится сигнатурный Id(по которому можно понять, какой у детали тип и кто её сделал) и порядковый Id, их сумма генерирует Id детали, которую пытается сделать поставщик. В данной программе один поставщик может сделать 1000 уникальных Id, после чего пойдёт по второму кругу.
  SupplierAll:
  Родительский класс для всех поставщиков при вызове MakeExample будет сгенерирована деталь с новым айди, после чего она вернёт эту деталь. Дочерние классы отличаются типом сгенерированной детали.
}
4)Запуска класса StartThreads с передачей в него инициализированной фабрики и выводного окна.
ViewBaze:
Класс, отвечающий за визуальную составляющую программы. В верхнем меня он генерирует кнопки, без прослушивания, через которые пользователь может следить за ходом работы программы(сколько деталей на складе, сколько всего их было произведено и сколько машин уже забрали). Так же наше окно имеет ползунки, отвечающие за скорость того или иного  производителя. Склад имеет доступ к этим функциям, и каждый раз, при производстве, запрашивает значение скорости в данный момент. Остальные функции обновляют данные. Они вызываются в складах.
StartThreads:
Класс инициализации потоков.
1)В визуальный класс передаётся этот класс, чтобы  можно было при закрытии окна убить все потоки
2)Создаются экземпляры класса ControlOfThread. ControlOfThread отвечает за запуск и прекращения работы задания, которые передаётся ему в конструкторе.{
  О заданиях:
  Все задания наследуются от класса TaskExamle. В самом TaskExamle есть несколько конструкторов, для разных типов потоков, и функция run, которая будет переопределена в каждом классе. Сам TaskExamle наследуется от Thread. 
  Классы для связи потоков и управления работой:
  Помимо складов, потоки имеют другие общие ресурсы.
  1) ForGetCar - связь между дилером и контроллером склада
  2)ForSuppliersCar - класс для связи между контролером и производителями машин. В нём хранится информация о том, сколько машин ещё   нужно произвести и сколько производятся в данный момент. Каждый раз, когда дилер забирает машину, контроллер перезаписывает значения этого класса.
  Так же, для контроля каждый поток имеет свой EndingSupport. В нём хранится логическая переменная, которая записывается в true по ходу исполнения потока, если поток был Interupted. 
 TaskForSupABE:
  В этом классе функция run переопределяется так, чтобы до бесконечности создавал деталь(Двигатель, аксессуар или корпус) и пытался положить эту деталь на склад. Каждый раз, когда он класть деталь на склад, он уведомляет.
  TaskForDiller:
  Определяет поведение потоков дилеров. В них создаётся логер(в каждом свой, но они используют один файл, не перезаписывая его, а добавляя). Дилер ожидает какое-то время, а затем пытается взять машину со склада. Затем он записывает это в лог( если использования логгера поставлено) или выводит в консоль(в противном случае), данные о машине.
  TaskForCarController:
  Его задача ожидать, пока дилер возьмет деталь, а потом перезаписать информацию для производителя.
  TaskForC:
  Бесконечно делает машины. Сначала он ожидает разрешения начать работу. После получения разрешения в ответственном за это классе появляется информация о том, что один из рабочих начал работу. Далее он последовательно запрашивает все нужные детали, добавляет их к машине , а затем добавляет на склад, после чего говорит ответственному классу, что он закончил.
}
Входные данные хранятся в файле out/production/CarFactory/configure/config.txt , настройки его интуитивно понятны.
