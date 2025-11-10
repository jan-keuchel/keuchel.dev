---
title: Embedded Software Labor
desc: Durchlauf des Labors des Moduls "Embedded Software"
date: 2025-11-09
logo: /assets/images/HKA.png
language: de
ongoing: true
---

## Kontext

Die Aufgabe ist, einen Decoder für ein Summensignal von GPS-Daten zu schreiben. Es gibt $24$ Satelliten. Hiervon senden $4$ Satelliten jeweils ein Bit. Aus diesen Daten ergibt sich ein Summensignal. Das Summensignal ist die Eingabe, aus welcher nun zurückgerechnet werden soll, welche $4$ Satelliten, welches Bite gesendet haben. 

Um ein Summensignal als Eingabe zu erhalten, wird dieses zuerst generiert. (Im Labor war dieses bereits als Eingabe gegeben.) 

---

## Theorie 

### Datenübertragung bei Satelliten 

Jeder Satellit hat eine eindeutige Chipsequenz. Möchte ein Satellit ein Bit $b=1$ senden, so sendet er seine Chipsequenz. Will er $b=0$ senden, sendet er das Inverse seine Chipsequenz -- also Nullen und Einsen geflippt.

Um Datenbits und diejenigen Bits zu unterscheiden, aus denen sich eine Chipsequenz zusammensetzt, werden erstere von nun an als "Bits" und letztere als "Chips" bezeichnet.

Ein Bit besteht aus $1023$ Chips. 

### Eigenschaften der Chipsequenzen

Die Chipsequenzen werden nun nicht als Bitstring der Länge $1023$ betrachtet, sondern als ein Vektor mit $1023$ Koordinaten, wobei alle Chips, die den Wert $0$ haben, zu einer Koordinate mit dem Wert $-1$ umgewandelt werden.
Chips mit einem Wert von $1$ bleiben erhalten.

Eine Chipsequenz $c$ kann um ein $\delta$ rotiert werden, was mit $c \ll \delta$  (linksrotation) dargestellt wird.
Um zu betrachten, wie zwei Chipsequenzen $c_i, c_j$ zueinander stehen, wird das sogenannte Korrelationsprodukt $\mathcal{CP}$ verwendet, was das Skalarprodukt zweier Chipsequenzen ist, wovon eine der beiden um ein $\delta$ rotiert ist:

$$
    \mathcal{CP}_{ij}(\delta) := c_i \cdot (c_j \ll \delta)
$$

Normanisiert man $\mathcal{CP}$, so erhält man einen Wert $\rho \in \[-1, 1\]$: den Korrelationskoeffizienten.

Des Weiteren sei das Autokorrelationsprodukt das Korrelationsprodukt mit $i=j$ und das Kreuzkorellationsprodukt das Korrelationsprodukt mit $i\ne j$.

Die Chipsequenzen der Satelliten sind sogenannte Goldfolgen, welche die Eigenschaft haben, dass das Kreuzkorellationsprodukt zweier Goldfolgen nur drei Werte annehmen kann.
Im Falle einer Chipsequenz welche durch eine gerade Anzahl an Schieberegistern generiert wird, sind diese gegeben durch:

$$
    \mathcal{CP}_{ij}(\delta) = 
    \begin{cases}
        -2^{\frac{n+2}{2}} - 1 & \text{in }12,5\% \text{ der Fälle} \\
        -1 & \text{in }75\% \text{ der Fälle}\\
        2^{\frac{n+2}{2}} - 1 & \text{in }12,5\% \text{ der Fälle}
    \end{cases}
$$

Für das Autokorrelationsprodukt zweier Goldfolgen kann man festhalten, dass

$$
    \mathcal{CP}_{ii}(\delta) = 
    \begin{cases}
        |c_i|  & \text{falls } \delta \equiv 0 \mod{|c_i|} \\
        -1 & \text{falls } \delta \not \equiv 0 \mod{|c_i|}\\
    \end{cases}
$$

In diesem Fall werden die Chipsequenzen mit Hilfe von $10$ Schieberegistern generiert.
Die Werte, die im Kreuzkorellationsprodukt entstehen können sind also $-65, -1$ und $63$.
Diese Werte sind im Verhältnis zum Ausschlag, der sich bei der Autokorrelation im Falle von $\delta \equiv 0 \mod{|c|}$ ergibt, sehr gering.

### Das Summensignal

Das Summensignal entsteht dadurch, dass mehrere Satelliten parallel und verschoben zueinander (asynchron) Bits -- also ihre Chipsequenzen -- senden.
Es ist also die Summe mehrerer, verschobener Chipsequenzen:

<div class="full-width-img img-theme-toggle">
    {% include lecture_data/embedded-software-lab/sumsignal_tex %}
</div>

Es wird nun die Annahme getroffen, dass ein Ausschnitt des Summensignals betrachtet wird, in dem alle Satelliten -- wenn diese denn etwas senden -- zyklisch das gleiche Bit senden.

Dies kann man sich im obigem Bild so vorstellen, dass die Daten der Satelliten $S_1$ bis $S_4$ jeweils an sich selbst angehängt -- bzw. vor sich selbst gehängt -- werden, sodass der rote Bereich vollständig mit Chips ausgefüllt ist.
Dann bildet man Spaltenweise die Summe und erhält das Summensignal $\mathcal{S}$.

{: .highlight-block .highlight-hint }
Eine andere Betrachtung ist die, dass man keine Summe über einem Ausschnitt mehrerer, an sich selbst angehängter Chipsequenzen bildet, sondern über mehreren, unterschiedlich rotierten Chipsequenzen.

Seien $d_j$ die vom Satelliten $S_j$ gesendeten Daten über denen das Summensignal $\mathcal{S}$ gebildet wird -- also entweder $c_j \ll \delta_j$ oder $\overline{c_j} \ll \delta_j$.
Bildet man das Korrelationsprodukt zwischen der Chipsequenz eines Satelliten $c_i$ und $\mathcal{S}$ so stellt man fest, dass 

$$
\begin{align*}
c_i \cdot \mathcal{S} &= c_i \cdot \sum_j d_i \\
                      &= c_i \cdot \sum_{j \neq i} d_j + c_i \cdot d_i \\
                      &= \alpha + c_i \cdot d_j \\
                      &= \alpha + \mathcal{CP}_{jj}(\delta_j) \\
                      &= \begin{cases}
                          |c_j| + \alpha & \text{falls } \delta \equiv 0 \pmod{|c_j|} \\[4pt]
                          -1 + \alpha   & \text{falls } \delta \not\equiv 0 \pmod{|c_j|}
                        \end{cases}
\end{align*}
$$

. Hierbei ist $\alpha$ ein Störfaktor, der durch die Daten der anderen Satelliten entstehen.

Da in diesem Fall nur $4$ Satelliten Daten senden und der Störwert pro Satellit maximal $65$ ist, der kleinst mögliche positive Peak bei $1023 - 3 \cdot 65 = 828$.
Der vom Betrag her kleinst mögliche negative Peak liebt bei $-1023 + 3 \cdot 65 = -828$.

## Erzeugen des Summensignals
Nun geht das darum, das Summensignal für zufällige Satelliten mit zufälligen Werten für gesendete Bits zu generieren.






{% comment %} ## Decoder in `C++` {% endcomment %}

{% comment %} ## Decoder in `C` {% endcomment %}

{% comment %} ## Vergleich und Optimierung {% endcomment %}
 
