Analiza cen mieszkań w Poznaniu (2010-2025): SQL, wizualizacje , wnioski

W pliku ceny_transakcyjne_poznan_features.xlsx znajdują się:
średnia kwartalna,
zmiana q/q (kwartał do kwartału),
zmiana r/r (rok do roku),
średnia krocząca 3-letnia (12 kwartałów),
minimum i maksimum globalne.

Wnioski z analizy
2010–2016 – względna stabilizacja z lekkimi spadkami.
2017–2021 – wyraźny i stabilny wzrost cen.
2022 – krótkotrwałe wyhamowanie.
2023–2025 – powrót silnego trendu wzrostowego.
Średnia krocząca wygładza szum i potwierdza długoterminowy trend rosnący.

Poniżej znajduje się zapytanie SQL, którego użyłem do wygenerowania finalnego pliku 'ceny_transakcyjne_poznan_features.xlsx'.

SELECT 
rok,
kw_num,
AVG(cena) AS srednia_kwartalna,
AVG(cena) - LAG(AVG(cena)) OVER (ORDER BY rok,kw_num) AS zmiana_q_q,
AVG(cena) - LAG(AVG(cena),4) OVER (ORDER BY rok,kw_num) AS zmiana_r_r,
AVG(cena) OVER (ORDER BY rok,kw_num ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS srednia_3letnia,
(SELECT MIN(cena) FROM ceny_mieszkan_poznan) AS min_cena_global,
(SELECT MAX(cena) FROM ceny_mieszkan_poznan) AS max_cena_global
FROM ceny_mieszkan_poznan
GROUP BY rok,kw_num
ORDER BY rok,kw_num;



Celem było:
przygotowanie danych o cenach mieszkań z raportu NBP,
oczyszczenie i ustrukturyzowanie danych,
obliczenie kluczowych wskaźników w SQL (trend, zmiany kwartalne, średnie kroczące),
przygotowanie wykresów i wniosków do raportu końcowego.

Projekt jest elementem nauki SQL + analizy danych.
