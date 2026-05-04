# Analiza modelu subskrypcyjnego KajoDataSpace

##  O projekcie
Projekt konkursowy dla **KajoData**, będący kompleksową analizą danych modelu subskrypcyjnego. Nazwałam go „Operacją na otwartym sercu”, ponieważ wymagał chirurgicznej precyzji w wydobywaniu wartości z bardzo ograniczonego zbioru danych.

**Wyzwanie:** Analiza została przeprowadzona przy użyciu zaledwie **3 kolumn danych**: `Data transakcji`, `Klient` oraz `Kwota`. Celem było sprawdzenie, jak wiele merytorycznych wniosków biznesowych można wyciągnąć z tak surowych informacji.

---
<img width="1785" height="995" alt="KajoData_dashboard1" src="https://github.com/user-attachments/assets/3b33cbb2-30bc-47e6-95d8-75b104371d93" />
<br><br>
<img width="1794" height="999" alt="KajoData_dashboard2" src="https://github.com/user-attachments/assets/00617efd-d1ff-4353-b8dc-fe186b8c723d" />
<br><br>
<img width="1775" height="984" alt="KajoData_dashboard3" src="https://github.com/user-attachments/assets/7594be34-7756-4e39-9ee1-7dbece25418f" />
<br><br>

---

##  Proces ETL & Przygotowanie danych

### Excel / Power Query
*   **Czyszczenie:** Standaryzacja typów danych, usuwanie zbędnych znaków.
*   **Przekształcanie:** Dodanie kolumn indeksu oraz wyodrębnienie miesiąca i roku transakcji, zmiana nazw kolumn i tabel na j.angielski.
*   **Agregacja:** Utworzenie tabeli `Order_count` zliczającej zamówienia dla każdego z 1 057 unikalnych klientów.

### Power BI & Modelowanie
*   **Kalendarz:** Implementacja tabeli `Calendar` dla zaawansowanych kalkulacji Time Intelligence.
*   **Analiza Kohortowa:** 
    *   Utworzenie tabeli `First_purchase` określającej datę pierwszego zamówienia dla każdego klienta.
    *   Wyliczenie `entry_price` (kwoty pierwszego zakupu) w celu identyfikacji pakietu wejściowego.
*   **Model danych:** Utworzenie modelu danych w schemacie gwiazdy.
*   **Data Mining:** Przeprowadziłam analizę korelacji między datami a średnią ceną zakupu. Dzięki zestawieniu tych danych z historią postów w mediach społecznościowych KajoData, zidentyfikowałam powtarzające się okresy promocyjne (Black Week, New Year's Special, March Offer, Last minute sale).
*   **Klasyfikacja:** Utworzenie zaawansowanej kolumny niestandardowej `promo_name` w Power Query, przypisującej transakcje do konkretnych kampanii marketingowych.

---

##  Kluczowe Miary (KPI) i logika biznesowa

W projekcie postawiłam na **dokładność statusową**, a nie transakcyjną, co jest kluczowe w modelu subskrypcyjnym:

*   **Active Customers:** Miara uwzględniająca datę ważności dostępu (biorąc pod uwagę datę transakcji oraz obliczonej na podstawie rodzaju subskrypcji i dołaczonego do modelu Kalendarza (Calendar)).
*   **Current MRR (Monthly Recurring Revenue):** Realna wartość przychodu powtarzalnego. Miara "rozbija" płatności roczne na 12 równych części, co eliminuje fałszywe skoki przychodów na wykresach i pokazuje realne tempo wzrostu. (możliwość zagłębiania się w poziom czasowy dzięki filtrowaniu po wybranej dacie)
*   **ARPU (Average Revenue Per User):** Wyliczane jako Current MRR / Active Customers. Wynik **163 zł** sugeruje pozycjonowanie produktu w segmencie premium.
*   **Churn Rate (14,77%):** Wyliczony z precyzją co do ID klienta przy użyciu funkcji `EXCEPT`, porównującej aktywność użytkowników w miesiącach następujących po sobie.
*   **LTV (Lifetime Value):** Na poziomie **1 107 zł**, co wskazuje, że średni cykl życia klienta wynosi ok. 7 miesięcy. Przy możliwości porównania z danymi o kosztach, można w przyszłości sprawdzać ile możemy wydać na pozyskanie jednego klienta, wiedząc ile średnio przynosi nam przychodu. 

---

##  Segmentacja klientów
Użytkownicy zostali podzieleni na trzy grupy na podstawie liczby transakcji:
1.  **VIP (> 5 transakcji):** Najmniejsza grupa (124 osoby), ale generująca najwyższe LTV (4 593 zł).
2.  **Loyal (2-5 transakcji):** Stabilna baza klientów.
3.  **One-Timers (1 transakcja):** Najliczniejsza grupa (279 osób). 
    *   *Insight:* Segment ten wymaga rozróżnienia na klientów "testowych" (miesięcznych) oraz rocznych, którzy nie dotarli jeszcze do 13. miesiąca (momentu odnowienia).

---

##  Rekomendacje strategiczne

1.  **Kampania Win-back (Miesiąc 11):** Intensywna promocja w 11. miesiącu subskrypcji, mająca na celu przekonanie klientów do pozostania w usłudze.
2.  **Program retencyjny VIP:** Skupienie się na segmencie VIP (wysoka wartość $LTV$) poprzez dedykowane, spersonalizowane oferty.
3.  **Konwersja klientów jednorazowych (One-Timers):** Program lojalnościowy – przekształcenie licznej grupy klientów „jednorazowych” w użytkowników „lojalnych” za pomocą zachęt pozakupowych.
4.  **Uproszczenie odnowień rocznych:** Uproszczenie procesu odnawiania subskrypcji rocznych, aby wyeliminować gwałtowny spadek na krzywej utrzymania klienta (Survival Curve).
5.  **Optymalizacja strategii promocyjnej:** Promocje z wysokimi rabatami przyciągają klientów jednorazowych, którzy szybko rezygnują z usług (churn). Należy położyć nacisk na wysokiej jakości onboarding zamiast agresywnej polityki cenowej, aby budować zdrowsze i bardziej stabilne kohorty długoterminowe.

---

##  Wnioski z analizy promocji
*   **Efekt ostatniej szansy:** Klienci najczęściej kupują w ostatnim dniu promocji – kluczowe jest stosowanie przypominajek „Last call”.
*   **Wzrost cen:** Informacja o planowanej podwyżce w marcu zadziałała motywująco, zwiększając sprzedaż droższych pakietów.
*   **Jakość vs Ilość:** Agresywne promocje (Black Week) zwiększają liczbę nowych klientów, ale często obniżają średnią jakość kohorty (wyższa podatność na churn).

---
*Analiza wykonana przez Agnieszkę Trzciałkowską | Maj 2026 | Dane: KajoData*
