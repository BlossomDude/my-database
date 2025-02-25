### Задержка и пропускная способность

**Задержка (Latency)** - это время, которое требуется для передачи одного запроса от клиента к серверу и обратно (или для выполнения одной единицы работы). Обычно она измеряется в миллисекундах (мс).

**Round Trip Time (RTT)** - общее время, которое требуется для отправки запроса на сервер и получения ответа. Иногда RTT используется как синоним задержки.

**Пропускная способность (Throughput)** - это количество запросов или единиц работы, которые система может обрабатывать в секунду. Обычно она измеряется в запросах в секунду (RPS) или транзакциях в секунду (TPS).

### Типы масштабирования

Масштабирование бывает 2-ух типов:

1. Вертикальное масштабирование
2. Горизонтальное масштабирование

###### Вертикальное масштабирование

Если мы **увеличим характеристики** _(оперативную память, хранилище, процессор)_ того же компьютера, чтобы он справлялся с большей нагрузкой, то это называется вертикальным масштабированием.

Этот тип масштабирования в основном используется в базах данных SQL, а также в приложениях с отслеживанием состояния, поскольку при горизонтальном масштабировании сложно поддерживать согласованность состояний.

###### Горизонтальное масштабирование

Вертикальное масштабирование невозможно после определённого момента. Мы не можем бесконечно увеличивать характеристики машины. Мы столкнёмся с узким местом, и после этого не сможем увеличивать характеристики.

Решение этой проблемы заключается в **добавлении новых устройств** и распределении поступающей нагрузки. Это называется горизонтальным масштабированием.

### CAP Теорема

Эта теорема описывает очень важный компромисс при проектировании любой системы.

Аббревиатура CAP состоит из 3 слов:

- **Согласованность:** каждый запрос на чтение возвращает один и тот же результат независимо от того, с какого узла мы считываем данные. Это означает, что все узлы одновременно имеют одни и те же данные. На приведенном выше рисунке вы можете видеть, что наш кластер баз данных согласован, поскольку все узлы имеют одни и те же данные.
- **Доступность:** система доступна и всегда может отвечать на запросы, даже если некоторые узлы выйдут из строя. Это означает, что даже если некоторые узлы выйдут из строя, система должна продолжать обслуживать запросы с помощью других работоспособных узлов.
- **Устойчивость к разделению:** система продолжает работать даже в случае сбоя связи или разделения сети между различными узлами.

###### Что такое теорема CAP

Теорема CAP гласит, что в распределённой системе можно одновременно гарантировать только два из этих трёх свойств. Все три свойства одновременно обеспечить невозможно.

- CA — возможно
- AP — возможно
- CP — возможно
- CAP — невозможно

Это вполне логично, если вдуматься.

В распределённой системе неизбежно возникают сбои в работе сети, поэтому система должна быть устойчива к таким сбоям. Это означает, что в распределённой системе «P» всегда будет присутствовать. Мы будем искать компромисс между CP и AP.