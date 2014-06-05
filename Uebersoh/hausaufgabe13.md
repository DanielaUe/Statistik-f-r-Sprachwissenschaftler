% Hausaufgabe 13
% Daniela Uebersohn <Uebersoh@students.uni-marburg.de>
% 2014-05-30
% 2014-05-31

Falls die Umlaute in dieser und anderen Dateien nicht korrekt dargestellt werden, sollten Sie File > Reopen with Encoding > UTF-8 sofort machen (und auf jeden Fall ohne davor zu speichern), damit die Enkodierung korrekt erkannt wird! 




# Die nächsten Punkte sollten langsam automatisch sein...
1. Kopieren Sie diese Datei in Ihren Ordner (das können Sie innerhalb RStudio machen oder mit Explorer/Finder/usw.) und öffnen Sie die Kopie. Ab diesem Punkt arbeiten Sie mit der Kopie. Die Kopie bitte `hausaufgabe13.Rmd` nennen und nicht `Kopie...`
2. Sie sehen jetzt im Git-Tab, dass der neue Ordner als unbekannt (mit gelbem Fragezeichen) da steht. Geben Sie Git Bescheid, dass Sie die Änderungen im Ordner verfolgen möchten (auf Stage klicken). Die neue Datei steht automatisch da.
3. Machen Sie ein Commit mit den bisherigen Änderungen (schreiben Sie eine sinnvolle Message dazu -- sinnvoll bedeutet nicht unbedingt lang) und danach einen Push.
4. Ersetzen Sie meinen Namen oben mit Ihrem. Klicken auf Stage, um die Änderung zu merken.
5. Ändern Sie das Datum auf heute. (Seien Sie ehrlich! Ich kann das sowieso am Commit sehen.)
6. Sie sehen jetzt, dass es zwei Symbole in der Status-Spalte gibt, eins für den Zustand im *Staging Area* (auch als *Index* bekannt), eins für den Zustand im Vergleich zum Staging Area. Sie haben die Datei modifiziert, eine Änderung in das Staging Area aufgenommen, und danach weitere Änderungen gemacht. Nur Änderungen im Staging Area werden in den Commit aufgenommen.
7. Stellen Sie die letzten Änderungen auch ins Staging Area und machen Sie einen Commit (immer mit sinnvoller Message!).
8. Vergessen Sie nicht am Ende, die Lizenz ggf. zu ändern!

Um einiges leichter zu machen, sollten Sie auch die Datei `priming.tab` aus dem Data-Ordner kopieren, stagen und commiten. Sie müssen ggf. Ihr Arbeitsverzeichnis setzen, wenn R die .tab-Datei nicht finden kann: Session > Set Working Directory > Source File Location

# Beschreibung des Experiments
- Aufgabe: Lexikalische Entscheidung 
- Abhängige Variable: Reaktionszeit, gemessen in Millisekunden (ms)
- Bedingungen (2 x 2)
    - `prime`: Englisch, Deutsch
    - `target`: Englisch, Deutsch
- Wiederholungen
    - Probanden `subj` ($n_1 = 30$)
    - Items `item` ($n_2=20$)

30 Subjects x 20 Items x 8 Bedingungen = 4800 Trials.


```r
priming <- read.table("Data/priming.tab", header = T)
```

```
## Warning: kann Datei 'Data/priming.tab' nicht öffnen: No such file or
## directory
```

```
## Error: kann Verbindung nicht öffnen
```

```r
priming$subj <- as.factor(priming$subj)
```

```
## Error: Objekt 'priming' nicht gefunden
```

```r
priming <- subset(priming, item <= 20)  # Filler ausschließen
```

```
## Error: Objekt 'priming' nicht gefunden
```

```r
priming$item <- as.factor(priming$item)
```

```
## Error: Objekt 'priming' nicht gefunden
```


## Darstellung der Daten ohne Berücksichtigung der Wiederholungen

```r
ggplot(data = priming) + geom_density(aes(x = RT, color = cond, fill = cond), 
    alpha = 0.3)
```

```
## Error: Objekt 'priming' nicht gefunden
```



```r
ggplot(data = priming) + geom_jitter(aes(x = cond, y = RT, color = cond, fill = cond), 
    alpha = 0.5)
```

```
## Error: Objekt 'priming' nicht gefunden
```



```r
ggplot(data = priming) + geom_violin(aes(x = cond, y = RT, color = cond, fill = cond), 
    alpha = 0.5)
```

```
## Error: Objekt 'priming' nicht gefunden
```



```r
ggplot(data = priming) + geom_boxplot(aes(x = cond, y = RT, color = cond, fill = cond), 
    alpha = 0.5)
```

```
## Error: Objekt 'priming' nicht gefunden
```


## Darstellung der Daten mit Berücksichtigung der Wiederholungen
Wenn wir naiv die Wiederholungen betrachten, können wir uns entweder auf den zufälligen Faktor **Subject** oder den zufälligen Faktor **Item** dadurch konzentrien, dass wir die Mittelwerte berechnen. 

### By Subject
Bei "Subject" berechnen wir die Mittelwerte aller Trials innerhalb einer Versuchsperson, d.h. wir nehmen den versuchpersonenweise Mittelwert über Items hinweg. Dafür können wir `aggregate()` nutzen.

```r
priming.by.subject <- aggregate(RT ~ cond * subj, data = priming, FUN = mean)
```

```
## Error: Objekt 'priming' nicht gefunden
```


Wir können die Tabelle, die durch entsteht, mit `xtable()` schön drucken. Bemerken Sie dabei, dass wir eine weitere Option (`results='asis'`) beim Code-Block setzen müssen!

```r
results = "asis"
print(xtable(priming.by.subject), type = "html", include.rownames = FALSE)
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```


Diese Tabelle ist etwas "lang" (ist ja in long format!). Wie könnten sie auch breiter machen mit der Funktion `dcast()` (`cast` für Data Frames) aus dem Paket `reshape2`. 

Mit Versuchspersonen als Zeilen und Bedingungen als Spalten:

```r
priming.by.subject.wide1 <- dcast(priming.by.subject, subj ~ cond, value.var = "RT")
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```

```r
print(xtable(priming.by.subject.wide1), type = "html", include.rownames = FALSE)
```

```
## Error: Objekt 'priming.by.subject.wide1' nicht gefunden
```

 
Mit Bedingungen als Zeilen und Versuchspersonen als Spalten:

```r
priming.by.subject.wide2 <- dcast(priming.by.subject, cond ~ subj, value.var = "RT")
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```

```r
print(xtable(priming.by.subject.wide2), type = "html", include.rownames = FALSE)
```

```
## Error: Objekt 'priming.by.subject.wide2' nicht gefunden
```


Welches Format macht am meisten Sinn?

Natürlich müssen wir auch die Daten grafisch darstellen:

```r
ggplot(data = priming.by.subject) + geom_density(aes(x = RT, color = cond, fill = cond), 
    alpha = 0.3)
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```

```r
ggplot(data = priming.by.subject) + geom_jitter(aes(x = cond, y = RT, color = cond, 
    fill = cond), alpha = 0.5)
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```

```r
ggplot(data = priming.by.subject) + geom_violin(aes(x = cond, y = RT, color = cond, 
    fill = cond), alpha = 0.5)
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```

```r
ggplot(data = priming.by.subject) + geom_boxplot(aes(x = cond, y = RT, color = cond, 
    fill = cond), alpha = 0.5)
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```


Sind alle Plots gleich gut?

### By Item
Bei "Item" berechnen wir die Mittelwerte aller Trials innerhalb eines Items, d.h. wir nehmen den itemweise Mittelwert über Versuchpersonen hinweg. Dafür können wir `aggregate()` nutzen.


```r
priming.by.item <- aggregate(RT ~ cond * item, data = priming, FUN = mean)
```

```
## Error: Objekt 'priming' nicht gefunden
```


Wir wollen auch hier die Daten tabellerisch darstellen. Erstellen Sie eine Tabelle in Wide-Format für die Mittelwerte by Item.
results='asis'
priming.by.item.wide1 <- dcast(priming.by.item,item ~ cond, value.var="RT")
print(xtable(priming.by.item.wide1),type="html",include.rownames=FALSE)
priming.by.item.wide2 <- dcast(priming.by.item,cond ~ item, value.var="RT")
print(xtable(priming.by.item.wide2),type="html",include.rownames=FALSE)

Und *eine* passende Grafik für die Daten by Item sollten wir auch generieren:
ggplot(data=priming.by.item) + geom_density(aes(x=RT,color=cond,fill=cond),alpha=0.3)

Sehen die Daten by Subject und by Item gleich aus? Wie sehen sie im Vergleich zu den Single-Trial-Daten aus?

# Subject- und Item-Analysen

## Subject-Analyse

```r
ggplot(data = priming) + geom_density(aes(x = RT, color = cond, fill = cond), 
    alpha = 0.1) + facet_wrap(~subj)
```

```
## Error: Objekt 'priming' nicht gefunden
```



```r
priming.f1 <- ezANOVA(priming, dv = .(RT), wid = .(subj), within = .(prime, 
    target), detailed = TRUE)
```

```
## Error: Objekt 'priming' nicht gefunden
```

```r
priming.f1
```

```
## Error: Objekt 'priming.f1' nicht gefunden
```


### ANOVA

```r
results = "asis"
print(xtable(priming.f1$ANOVA), type = "html", include.rownames = FALSE)
```

```
## Error: Objekt 'priming.f1' nicht gefunden
```


Leider sehen die Zahlen weniger als optimal aus -- es gibt Kommastellen bei Ganzzahlen und keine nicht-Null-Stellen bei manchen kleinen Kommazahlen. Dafür gibt es auch Optionen für `xtable`: `digits` (Stellen) und `display` (Darstellungsart)


```r
# xtable braucht immer eine 'extra' Angabe bei digits und display für die
# Row-Namen, auch wenn Sie nicht gedruckt werden s: string, d: digit, f:
# float (Gleitkommazahl), e: exponent, g: exponent, nur wenn nötig, fg:
# float mit nicht-Null-Stellen bei floats: digits = Anzahl Stellen *nach*
# dem Komma
print(xtable(priming.f1$ANOVA, display = c("s", "s", "d", "d", "f", "f", "f", 
    "fg", "s", "g"), digits = c(0, 0, 0, 0, 2, 2, 2, 2, 0, 2)), type = "html", 
    include.rownames = FALSE)
```

```
## Error: Objekt 'priming.f1' nicht gefunden
```


Sie fragen sich evtl, woher der weitere Faktor `(Intercept)` kommt. Intercept ist der *Abschnitt* und beschreibt, wie weit weg die Basis des Models von Null ist -- hier ist sie ziemlich weit weg von Null, was Sinn macht, weil niemand eine Reaktionszeit von Null hat! 

### Spherizität
Die jeweiligen Faktoren haben alle zwei Stufen, also weniger als drei -- Spherizität ist kein Problem!

## Item-Analyse

```r
ggplot(data = priming) + geom_density(aes(x = RT, color = cond, fill = cond), 
    alpha = 0.1) + facet_wrap(~item)
```

```
## Error: Objekt 'priming' nicht gefunden
```

Führen Sie die entsprechende Item-Analyse aus.
priming.f1 <- ezANOVA(priming
                      , dv=.(RT)
                      , wid=.(item)
                      , within=.(prime,target)
                      , detailed=TRUE
                      )
priming.f1
# Interaktionen 
In der Analyse by Subjects haben wir eine Interaktion zwsichen `prime` und `target`. Solche Interaktionen berichtet man in der Regel nur, wenn sie auch in der Auflösung zu finden ist. Hier heißt das, dass wir die Daten innerhalb der verschiedenen Stufen eines Faktors anschauen und weitere ANOVAs berechnen, um zu schauen, ob der verbleibende Faktor (oder die verbleibenden Faktoren bei mehrfaktoriellen ANOVA) noch signifikant wird. Wenn er in keiner Stufe des anderen Faktors signifikant wird, betrachten wir die Interaktion als nicht bedeutsam.

Es gibt keine statistische Basis, nach der wir entscheiden können, welchen Faktor aufzulösen. Die Entscheidung müssen wir *a priori* anhand unserer Hypothesen machen. Wenn wir in beide Richtungen auflösen und schauen würden, welche Richtung die besseren Ergebnisse ergibt, sündigen wir wieder mit multiplem Testen!

Bei der Studie hier haben wir als Fragestellung für das Gesamtexperiment, ob Priming gleich gut sprachübergreifend als sprachintern funktioniert. Wenn wir überlegen, wie wir die Interaktion auflösen möchten, können wir uns weitere (Unter)Fragestellungen vorstellen:

1. *Hat die Sprache des Primes eine Auswirkung auf die Stärke des Priming-Effekts?* In diesem Fall wollen wir dann `target` festhalen bzw. auflösen, damit wir `prime` noch variieren können, d.h. die Wirkung von `prime` unter gewissen Umständen (Zielsprache) untersuchen.
2. *Hat die Sprache des Targets eine Auswirkung auf die Stärke des Priming-Effekts?*  In diesem Fall wollen wir dann `prime` festhalen bzw. auflösen, damit wir `target` noch variieren können, d.h. die Wirkung von `target` unter gewissen Umständen (Priming-Sprache) untersuchen.

Hier möchte ich wissen, ob Primes in der Fremdsprache die gleiche Wirkung haben wie Primes in der Muttersprache. Das heißt, ich möchte `prime` variieren, ich muss also `target` stufenweise festhalten bzw. auflösen.

Eine Grafik zur Auflösung:

```r
ggplot(data = priming) + geom_boxplot(aes(x = prime, y = RT), alpha = 0.45) + 
    facet_wrap(~target) + ggtitle("Priming aufgelöst nach target -- Single-Trial-Daten")
```

```
## Error: Objekt 'priming' nicht gefunden
```



```r
priming.by.subject <- aggregate(RT ~ target * prime * subj, data = priming, 
    FUN = mean)
```

```
## Error: Objekt 'priming' nicht gefunden
```

```r
ggplot(data = priming.by.subject) + geom_boxplot(aes(x = prime, y = RT), alpha = 0.45) + 
    facet_wrap(~target) + ggtitle("Priming aufgelöst nach target -- Mean-by-Subject-Daten")
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```


Um die Auflösung zu machen, nutzen wir `subset()`. Dabei müssen wir auch beachten, dass `target` nicht mehr als Faktor in der ANOVA auftaucht, denn wir berechnen die ANOVA innerhalb der Stufen von `target`!

# Englisches Target

```r
priming.f1.englisch.target <- ezANOVA(subset(priming, target == "E"), dv = .(RT), 
    wid = .(subj), within = .(prime), detailed = TRUE)
```

```
## Error: Objekt 'priming' nicht gefunden
```

```r
print(xtable(priming.f1.englisch.target$ANOVA, display = c("s", "s", "d", "d", 
    "f", "f", "f", "fg", "s", "g"), digits = c(0, 0, 0, 0, 2, 2, 2, 2, 0, 2)), 
    type = "html", include.rownames = FALSE)
```

```
## Error: Objekt 'priming.f1.englisch.target' nicht gefunden
```



# Deutsches Target
Führen Sie die entsprechende Analyse für deutsches Target aus.
results='asis'
priming.f1.deutsch.target <- ezANOVA(subset(priming, target == "D")
                                      , dv=.(RT)
                                      , wid=.(subj)
                                      , within=.(prime)
                                      , detailed=TRUE
                                      )
print(xtable(priming.f1.deutsch.target$ANOVA
             , display=c("s","s","d","d","f","f","f","fg","s","g")
             , digits=c(0,0,0,0,2,2,2,2,0,2))
      ,type="html",include.rownames=FALSE)


# Lizenz
Diese Datei dient nur Prüfungszwecken.