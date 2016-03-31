Die Datenbank-Transaktionseinheit (Database Transaction Unit, DTU) ist die Maßeinheit in SQL-Datenbank, die die relative Leistung von Datenbanken basierend auf einer echten Maßeinheit darstellt: die Datenbanktransaktion. Dazu haben wir eine Reihe von Vorgängen, die typisch für einen online Transaction processing (OLTP)-Anforderung, und klicken Sie dann gemessen, wie viele Transaktionen pro Sekunde unter vollständig abgeschlossen werden konnte, Startbedingungen geladen (, die die Kurzversion ist, können Sie die komplizierten Details im Lesen der [Benchmark-Übersicht](../articles/sql-database/sql-database-benchmark-overview.md)). 

Eine Basic-Datenbank verfügt über 5 DTUs, was bedeutet, dass 5 Transaktionen pro Sekunde abgeschlossen werden können, wohingegen eine Premium-P11-Datenbank über 1.750 DTUs verfügt. 

![Einführung in SQL-Datenbank: Einzeldatenbank-DTUs nach Tarif und Stufe.](./media/sql-database-understanding-dtus/single_db_dtus.png)

### DTU im Vergleich zu eDTU

Die DTU für Einzeldatenbanken kann direkt in die eDTU für elastische Datenbanken übertragen werden. Beispielsweise bietet eine Datenbank in einem Pool mit elastischen Basic-Datenbanken bis zu 5 eDTUs. Das ist eine ähnliche Leistung wie bei einer Basic-Einzeldatenbank. Der Unterschied besteht darin, dass elastische Datenbanken keine eDTUs aus dem Pool verwenden, bis dies erforderlich ist. 

![Einführung in SQL-Datenbank: Elastische Pools nach Tarif.](./media/sql-database-understanding-dtus/sqldb_elastic_pools.png)

Ein einfaches Beispiel veranschaulicht dies. Nehmen Sie einen elastischen Basic-Datenbankpool mit 1.000 DTUs, in dem Sie 800 Datenbanken ablegen. Solange nur 200 der 800 Datenbanken zu einem beliebigen Zeitpunkt genutzt werden (5 DTU X 200 = 1.000), wird die maximale Kapazität des Pools nicht erreicht wird und die Leistung der Datenbank nicht beeinträchtigt. In diesem Beispiel wird aus Gründen der Übersichtlichkeit vereinfacht. In Wirklichkeit sind die Dinge ein wenig komplizierter. Das Portal erledigt die komplizierten Aufgaben für Sie und gibt eine Empfehlung basierend auf der bisherigen Verwendung der Datenbank ab. Finden Sie unter [Überlegungen zum Preis und Leistung für einen elastischen Pool](../articles/sql-database/sql-database-elastic-pool-guidance.md) erfahren, wie die Empfehlungen funktionieren oder selbst dafür. 


