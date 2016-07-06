---
title: "CPUSets für die Entwicklung von Spielen"
description: "Dieser Artikel enthält eine Übersicht über die CPUSets-API,die in der universellen Windows-Plattform (UWP) neu ist, und liefert grundlegende Informationen zur Entwicklung von Spielen und Anwendungen."
author: hammondsp
ms.sourcegitcommit: 3cefaf4e527d2a0da412dab474a348b55ad409c9
ms.openlocfilehash: f125ae7e268a8d35b477a1557c498762869f859b

---

# CPUSets für die Entwicklung von Spielen

## Einführung

Die universelle Windows-Plattform (UWP) ist das Herzstück einer Vielzahl von elektronischen Geräten für Verbraucher. Als solches benötigt sie eine allgemeine API, um die Bedürfnisse aller Anwendungsarten zu erfüllen: von Spielen über eingebettete Apps bis hin zu Enterprise-Software, die auf Servern ausgeführt wird. Durch die Nutzung der richtigen Informationen, die von der API bereitgestellt werden, können Sie sicherstellen, dass Ihr Spiel auf jeder Hardware optimal ausgeführt wird.

## CPUSets-API

Die CPUSets-API bietet Kontrolle darüber, welche CPU-Sätze zur Verfügung stehen, um die Ausführung von Threads darauf zu planen. Zwei Funktionen sind verfügbar, um zu steuern, wo Threads geplant werden:
- **SetProcessDefaultCpuSets**: Diese Funktion kann zum Angeben der CPU-Sätze verwendet werden, auf denen neue Threads ausgeführt werden können, wenn sie nicht bestimmten CPU-Sätze zugeordnet sind.
- **SetThreadSelectedCpuSets**: Mit dieser Funktion können Sie die CPU-Sätze beschränken, auf denen ein bestimmter Thread ausgeführt wird.

Wenn die Funktion **SetProcessDefaultCpuSets** niemals verwendet wird, können neu erstellte Threads auf jeder CPU geplant werden, die für Ihren Prozess verfügbar ist. Dieser Abschnitt behandelt die Grundlagen der CPUSets-API.

### GetSystemCpuSetInformation

Die erste API, die zum Sammeln von Informationen verwendet wird, ist die **GetSystemCpuSetInformation**-Funktion. Diese Funktion füllt Informationen in einem Bereich von **SYSTEM_CPU_SET_INFORMATION**-Objekten auf, die vom Titel-Code bereitgestellt werden. Der Speicher für das Ziel muss vom Spielcode zugeordnet werden, dessen Größe durch Aufrufen von **GetSystemCpuSetInformation** selbst bestimmt wird. Dies erfordert zwei Aufrufe von **GetSystemCpuSetInformation**, wie im folgenden Beispiel gezeigt.

```
unsigned long size;
HANDLE curProc = GetCurrentProcess();
GetSystemCpuSetInformation(nullptr, 0, &size, curProc, 0);

std::unique_ptr<uint8_t[]> buffer(new uint8_t[size]);

PSYSTEM_CPU_SET_INFORMATION cpuSets = reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(buffer.get());
  
GetSystemCpuSetInformation(cpuSets, size, &size, curProc, 0);
```

Jede Instanz von zurückgegebenen **SYSTEM_CPU_SET_INFORMATION** enthält Informationen zu einer eindeutigen Verarbeitungseinheit, die auch als CPU-Satz bezeichnet wird. Dies bedeutet nicht notwendigerweise, dass er ein eindeutiges physisches Hardwaregerät darstellt. CPUs, die Hyperthreading nutzen, verfügen über mehrere logische Kerne, die auf einem einzigen physischen Verarbeitungskern ausgeführt werden. Das Planen von mehreren Threads auf verschiedenen logischen Kernen, die sich auf dem gleichen physischen Kern befinden, ermöglicht eine Optimierung von Ressourcen auf Hardware-Ebene, für die andernfalls zusätzliche Arbeit auf der Kernel-Ebene durchgeführt werden müsste. Zwei Threads, die auf separaten logischen Kernen auf dem gleichen physischen Kern geplant sind, müssen die CPU-Zeit teilen, würden aber effizienter ausgeführt werden, als wenn sie auf dem gleichen logischen Kern geplant worden wären.

### SYSTEM_CPU_SET_INFORMATION

Die Informationen in jeder Instanz dieser Datenstruktur, die von **GetSystemCpuSetInformation** zurückgegeben wird, enthalten Informationen zu einer eindeutigen Verarbeitungseinheit, auf der Threads geplant werden können. Angesichts der möglichen Bandbreite von Zielgeräten gelten möglicherweise viele der Informationen in der **SYSTEM_CPU_SET_INFORMATION**-Datenstruktur nicht für die Entwicklung von Spielen. Tabelle 1 enthält eine Erläuterung der Datenmember, die für die Entwicklung von Spielen hilfreich sind.

 **Tabelle 1. Für die Entwicklung von Spielen hilfreiche Datenmember.**

| Membername  | Datentyp | Beschreibung |
| ------------- | ------------- | ------------- |
| Typ  | CPU_SET_INFORMATION_TYPE  | Der Typ der Informationen in der Struktur. Wenn der Wert nicht **CpuSetInformation** lautet, sollte er ignoriert werden.  |
| ID  | ohne Vorzeichen lang  | Die ID des angegebenen CPU-Satzes. Dies ist die ID, die mit CPU-Satz-Funktionen wie **SetThreadSelectedCpuSets** verwendet werden sollte.  |
| Gruppe  | ohne Vorzeichen kurz  | Gibt die „Prozessorgruppe“ des CPU-Satzes an. Mit Prozessorgruppen kann ein PC mehr als 64 logische Prozessorkerne haben, und ein Austausch von CPUs per Hot-Swap bei laufendem System wird möglich. Es kommt nicht oft vor, dass ein PC kein Server mit mehr als einer Gruppe ist. Wenn Sie nicht gerade Anwendungen schreiben, die auf großen Servern oder Serverfarmen ausgeführt werden sollen, empfiehlt es sich, CPU-Sätze in einer einzelnen Gruppe zu verwenden, da die meisten Verbraucher-PCs nur eine Prozessorgruppe haben. Alle anderen Werte in dieser Struktur beziehen sich auf die Gruppe.  |
| LogicalProcessorIndex  | char-Wert ohne Vorzeichen  | Zur Gruppe relativer Index des CPU-Satzes  |
| CoreIndex  | char-Wert ohne Vorzeichen  | Zur Gruppe relativer Index des physischen CPU-Kerns, auf dem sich der CPU-Satz befindet.  |
| LastLevelCacheIndex  | char-Wert ohne Vorzeichen  | Zur Gruppe relativer Index des letzten Caches, der diesem CPU-Satz zugeordnet ist. Dies ist der langsamste Cache, es sei denn, das System verwendet NUMA-Knoten, in der Regel den L2- oder L3-Cache.  |

<br />

Die anderen Datenmember liefern Informationen, von denen es unwahrscheinlich ist, dass sie CPUs in Verbrauchercomputern oder anderen Verbrauchergeräten beschreiben, sodass sie wahrscheinlich nicht hilfreich sein. Die von den zurückgegebenen Daten gelieferten Informationen können dann verwendet werden, um Threads auf verschiedene Weise zu organisieren. Im Abschnitt [Überlegungen für die Spieleentwicklung](#considerations-for-game-development) dieses Whitepapers sind verschiedene Möglichkeiten beschrieben, wie diese Daten zur Optimierung der Thread-Zuordnung genutzt werden können.

Im Folgenden sind einige Beispiele für die Art der Informationen aufgeführt, die von UWP-Anwendungen gesammelt werden, die auf verschiedene Arten von Hardware ausgeführt werden.

**Tabelle 2. Informationen, die von einer UWP-App zurückgegeben werden, die auf einem Microsoft Lumia 950 ausgeführt wird. Dies ist ein Beispiel für ein System, das über mehrere Caches der letzten Ebene verfügt. Das Lumia 950 bietet einen Qualcomm 808 Snapdragon-Prozess, der einen Dual-Core ARM Cortex A57 und Quad-Core ARM Cortex A53-CPUs enthält.**

  ![Tabelle 2](images/cpusets-table2.png)

**Tabelle 3. Informationen, die von einer UWP-App zurückgegeben werden, die auf einem herkömmlichen PC ausgeführt wird. Dies ist ein Beispiel für ein System, das Hyperthreading verwendet; jeder physische Kern verfügt über zwei logische Kerne, auf denen Threads geplant werden können. In diesem Fall enthielt das System eine Intel Xenon CPU E5-2620.**

  ![Tabelle 3](images/cpusets-table3.png)

**Tabelle 4. Informationen, die von einer UWP-App zurückgegeben werden, die auf einem Quad-Core Microsoft Surface Pro 4 ausgeführt wird. Diesem System verfügt über eine Intel Core i5 6300-CPU.**

  ![Tabelle 4](images/cpusets-table4.png)

### SetThreadSelectedCpuSets

Nachdem nun Informationen zu den CPU-Sätzen verfügbar sind, können sie zum Organisieren von Threads verwendet werden. Das Handle eines mit **CreateThread** erstellten Threads wird an diese Funktion übergeben, zusammen mit einem Bereich von IDs der CPU-Sätze, auf denen der Thread geplant werden kann. Ein Beispiel für die Nutzung wird im folgenden Code veranschaulicht.

```
HANDLE audioHandle = CreateThread(nullptr, 0, AudioThread, nullptr, 0, nullptr);
unsigned long cores [] = { cpuSets[0].CpuSet.Id, cpuSets[1].CpuSet.Id };
SetThreadSelectedCpuSets(audioHandle, cores, 2);
```
In diesem Beispiel wird ein Thread basierend auf einer Funktion erstellt, die als **AudioThread** deklariert wird. Dieser Thread kann dann auf einem von zwei CPU-Sätzen geplant werden. Threadbesitz des CPU-Satzes ist nicht ausschließend. Threads, die erstellt werden, ohne an einen bestimmten CPU-Satz gebunden zu sein, verwenden möglicherweise Zeit vom **AudioThread**. Ebenso können andere erstellte Threads zu einem späteren Zeitpunkt auch an einen oder beide dieser CPU-Sätze gebunden sein.

### SetProcessDefaultCpuSets

Die Umkehrung zu **SetThreadSelectedCpuSets** ist **SetProcessDefaultCpuSets**. Bei der Erstellung von Threads müssen diese nicht an bestimmte CPU-Sätze gebunden werden. Wenn Sie nicht möchten, dass diese Threads auf bestimmten CPU-Sätzen ausgeführt werden (z. B. auf von Ihrem Render-Thread oder Audio-Thread verwendeten CPU-Sätzen), können Sie diese Funktion verwenden, um anzugeben, auf welchen Kernen diese Threads geplant werden dürfen.

## Überlegungen für die Spieleentwicklung

Wie wir bereits gesehen haben, bietet die CPUSets-API viele Informationen und Flexibilität rund um die Planung von Threads. Anstatt nach dem Bottom-up-Konzept zu versuchen, Anwendungsfälle für diese Daten zu finden, ist es effektiver, den Top-Down-Ansatz zu verwenden, bei dem ermittelt wird, wie die Daten für gängige Szenarien verwendet werden können.

### Arbeiten mit zeitkritischen Threads und Hyperthreading

Diese Methode ist effektiv, wenn Ihr Spiel einige Threads aufweist, die in Echtzeit zusammen mit anderen Arbeitsthreads ausgeführt werden müssen, die relativ wenig CPU-Zeit in Anspruch nehmen. Manche Aufgaben, z. B. fortlaufende Hintergrundmusik, müssen für ein optimales Spielerlebnis ohne Unterbrechung ausgeführt werden. Bereits ein einzelner Frame mit Audio-Threadzurückstellung kann eine Störung verursachen, sodass es wichtig ist, dass für jeden Frame die erforderliche Menge an CPU-Zeit zur Verfügung steht.

Mithilfe von **SetThreadSelectedCpuSets** in Verbindung mit **SetProcessDefaultCpuSets** können Sie sicherstellen, dass Ihre rechenintensiven Threads nicht von Arbeitsthreads unterbrochen werden. **SetThreadSelectedCpuSets** können verwendet werden, um Ihre rechenintensiven Threads bestimmten CPU-Sätzen zuzuweisen. **SetProcessDefaultCpuSets** kann dann verwendet werden, um sicherzustellen, dass alle erstellten nicht zugewiesenen Threads auf anderen CPU-Sätzen geplant werden. Im Fall von CPUs, die Hyperthreading nutzen, ist es auch wichtig, logische Kerne auf dem gleichen physischen Kern zu berücksichtigen. Arbeitsthreads sollten nicht auf logischen Kernen ausgeführt werden dürfen, die den gleichen physischen Kern wie ein Thread verwenden, den Sie mit Echtzeit-Reaktionsfähigkeit ausführen möchten. Der folgende Code veranschaulicht, wie Sie bestimmen, ob ein PC Hyperthreading verwendet.

```
unsigned long retsize = 0;
(void)GetSystemCpuSetInformation( nullptr, 0, &retsize,
    GetCurrentProcess(), 0);
 
std::unique_ptr<uint8_t[]> data( new uint8_t[retsize] );
if ( !GetSystemCpuSetInformation(
    reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>( data.get() ),
    retsize, &retsize, GetCurrentProcess(), 0) )
{
    // Error!
}
 
std::set<DWORD> cores;
std::vector<DWORD> processors;
uint8_t const * ptr = data.get();
for( DWORD size = 0; size < retsize; ) {
    auto info = reinterpret_cast<const SYSTEM_CPU_SET_INFORMATION*>( ptr );
    if ( info->Type == CpuSetInformation ) {
         processors.push_back( info->CpuSet.Id );
         cores.insert( info->CpuSet.CoreIndex );
    }
    ptr += info->Size;
    size += info->Size;
}
 
bool hyperthreaded = processors.size() != cores.size();
```

Wenn das System Hyperthreading verwendet, ist es wichtig, dass der Satz von Standard-CPU-Sätzen keine logischen Kerne auf dem gleichen physischen Kern wie Echtzeit-Threads enthält. Wenn das System kein Hyperthreading verwendet, muss nur sichergestellt werden, dass die CPU-Standardsätze nicht den gleichen Kern wie der CPU-Satz enthält, der Ihren Audio-Thread ausführt.

Ein Beispiel für das Organisieren von Threads basierend auf physischen Kernen finden Sie im CPUSets-Beispiel, das im GitHub-Repository verfügbar ist, das im Abschnitt [Zusätzliche Ressourcen](#additional-resources) verlinkt ist.

### Senken der Kosten der Cache-Kohärenz mit Cache der letzten Ebene

Cache-Kohärenz bedeutet, dass gecachter Arbeitsspeicher der gleiche für mehrere Hardwareressourcen ist, die auf dieselben Daten zugreifen. Wenn Threads auf verschiedenen Kernen geplant sind, aber auf dieselben Daten zugreifen, arbeiten sie möglicherweise mit separaten Kopien dieser Daten in verschiedenen Caches. Um richtige Ergebnisse zu erhalten, muss die Kohärenz dieser Caches gewährleistet sein. Die Aufrechterhaltung der Kohärenz zwischen mehreren Caches ist relativ teuer, ist aber erforderlich, damit ein System mit mehreren Kernen ausgeführt werden kann. Darüber hinaus liegt es völlig außerhalb der Kontrolle des Client-Codes; das zugrunde liegende System arbeitet unabhängig daran, Caches auf dem neuesten Stand zu halten, indem es auf zwischen Kernen freigegebene Speicherressourcen zugreift.

Wenn Ihr Spiel mehrere Threads verwendet, die gemeinsam eine besonders große Menge an Daten nutzen, können Sie die Kosten der Cache-Kohärenz minimieren, indem Sie sicherstellen, dass sie auf CPU-Sätzen geplant werden, die einen Cache der letzten Ebene teilen. Der Cache der letzten Ebene ist der langsamste Cache, der für einen Kern auf Systemen zur Verfügung steht, die keine NUMA-Knoten verwenden. Es kommt äußerst selten vor, dass ein Spiele-PC NUMA-Knoten nutzt. Wenn Kerne keinen Cache der letzten Ebene teilen, müsste zur Aufrechterhaltung der Kohärenz auf Speicherressourcen höherer Ebene und somit langsamere Speicherressourcen zugegriffen werden. Durch eine Bindung von zwei Threads an separate CPU-Sätze, die einen Cache und einen physischen Kern teilen, kann eine noch bessere Leistung erzielt werden, als durch deren Planung auf separaten physischen Kernen, wenn sie in einem beliebigen Frame nicht mehr als 50 % der Zeit benötigen. 

In diesem Codebeispiel wird veranschaulicht, wie Sie ermitteln, ob Threads, die häufig kommunizieren, einen Cache der letzten Ebene teilen können.

```
unsigned long retsize = 0;
(void)GetSystemCpuSetInformation(nullptr, 0, &retsize,
    GetCurrentProcess(), 0);
 
std::unique_ptr<uint8_t[]> data(new uint8_t[retsize]);
if (!GetSystemCpuSetInformation(
    reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(data.get()),
    retsize, &retsize, GetCurrentProcess(), 0))
{
    // Error!
}
 
unsigned long count = retsize / sizeof(SYSTEM_CPU_SET_INFORMATION);
bool sharedcache = false;
 
std::map<unsigned char, std::vector<SYSTEM_CPU_SET_INFORMATION>> cachemap;
for (size_t i = 0; i < count; ++i)
{
    auto cpuset = reinterpret_cast<PSYSTEM_CPU_SET_INFORMATION>(data.get())[i];
    if (cpuset.Type == CPU_SET_INFORMATION_TYPE::CpuSetInformation)
    {
        if (cachemap.find(cpuset.CpuSet.LastLevelCacheIndex) == cachemap.end())
        {
            std::pair<unsigned char, std::vector<SYSTEM_CPU_SET_INFORMATION>> newvalue;
            newvalue.first = cpuset.CpuSet.LastLevelCacheIndex;
            newvalue.second.push_back(cpuset);
            cachemap.insert(newvalue);
        }
        else
        {
            sharedcache = true;
            cachemap[cpuset.CpuSet.LastLevelCacheIndex].push_back(cpuset);
        }
    }
}
```

Das in Abbildung 1 dargestellte Cache-Layout ist ein Beispiel für die Art von Layout, die Sie möglicherweise bei einem System sehen. In dieser Abbildung sehen Sie eine Darstellung der Caches in einem Microsoft Lumia 950. Threadübergreifende Kommunikation zwischen CPU 256 und CPU 260 würde erheblichen Overhead verursachen, da das System seine L2-Caches kohärent halten müsste.

**Abbildung 1. Cache-Architektur in einem Microsoft Lumia 950-Gerät.**

![Lumia 950-Cache](images/cpusets-lumia950cache.png)

## Zusammenfassung

Die für UWP-Entwicklung verfügbare CPUSets-API bietet eine beträchtliche Menge an Informationen und Kontrolle über Ihre Multithreading-Optionen. Der zusätzliche Komplexität im Vergleich zu früheren Multithread-APIs für die Windows-Entwicklung ist mit einer Lernkurve verbunden. Die gestiegene Flexibilität ermöglicht aber letztendlich eine bessere Leistung auf unterschiedlichen Verbraucher-PCs und anderen Hardwarezielen.

## Weitere Ressourcen
- [CPU-Sätze (MSDN)](https://msdn.microsoft.com/library/windows/desktop/mt186420(v=vs.85).aspx)
- [Von ATG bereitgestelltes CPUSets-Beispiel](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/CPUSets)




<!--HONumber=Jun16_HO5-->


