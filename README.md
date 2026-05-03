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

##  Proces ETL & Przygotowanie Danych

### Excel / Power Query
*   **Cleaning:** Standaryzacja typów danych, usuwanie zbędnych spacji (trimming) i czyszczenie rekordów.
*   **Feature Engineering (Tabela Sale):** Dodanie kolumn indeksu oraz wyodrębnienie miesiąca i roku transakcji.
*   **Agregacja:** Utworzenie tabeli `Order_count` zliczającej zamówienia dla każdego z 1 057 unikalnych klientów.

### Power BI & Modelowanie
*   **Kalendarz:** Implementacja tabeli `Calendar` dla zaawansowanych kalkulacji Time Intelligence.
*   **Analiza Kohortowa:** 
    *   Utworzenie tabeli `First_purchase` określającej datę pierwszego zamówienia dla każdego klienta.
    *   Wyliczenie `entry_price` (kwoty pierwszego zakupu) w celu identyfikacji pakietu wejściowego.
*   **Data Mining (Kopalnia informacji):** Przeprowadziłam analizę korelacji między datami a średnią ceną zakupu. Dzięki zestawieniu tych danych z historią postów w mediach społecznościowych KajoData, zidentyfikowałam okresy promocyjne (Black Week, New Year's Special, March Offer).
*   **Klasyfikacja:** Utworzenie zaawansowanej kolumny niestandardowej `promo_name` w Power Query, przypisującej transakcje do konkretnych kampanii marketingowych.

---

##  Kluczowe Miary (KPI) i Logika Biznesowa

W projekcie postawiłam na **dokładność statusową (Status-based)**, a nie transakcyjną, co jest kluczowe w modelu subskrypcyjnym:

*   **Active Customers:** Miara uwzględniająca datę ważności dostępu (Monthly: 31 dni, Semi-annual: 183 dni, Annual: 366 dni).
*   **Current MRR (Monthly Recurring Revenue):** Realna wartość przychodu powtarzalnego. Miara "rozbija" płatności roczne na 12 równych części, co eliminuje fałszywe skoki przychodów na wykresach i pokazuje realne tempo wzrostu.
*   **ARPU (Average Revenue Per User):** Wyliczane jako $Current MRR / Active Customers$. Wynik **163 zł** sugeruje pozycjonowanie produktu w segmencie premium.
*   **Churn Rate (14,77%):** Wyliczony z precyzją co do ID klienta przy użyciu funkcji `EXCEPT`, porównującej aktywność użytkowników w miesiącach następujących po sobie.
*   **LTV (Lifetime Value):** Na poziomie **1 107 zł**, co wskazuje, że średni cykl życia klienta wynosi ok. 7 miesięcy.

---

##  Segmentacja Klientów
Użytkownicy zostali podzieleni na trzy grupy na podstawie liczby transakcji:
1.  **VIP (> 5 transakcji):** Najmniejsza grupa (124 osoby), ale generująca najwyższe LTV (4 593 zł).
2.  **Loyal (2-5 transakcji):** Stabilna baza klientów.
3.  **One-Timers (1 transakcja):** Najliczniejsza grupa (279 osób). 
    *   *Insight:* Segment ten wymaga rozróżnienia na klientów "testowych" (miesięcznych) oraz rocznych, którzy nie dotarli jeszcze do 13. miesiąca (momentu odnowienia).

---

##  Rekomendacje Strategiczne

1.  **Kampania Win-back (Miesiąc 11):** Automatyzacja komunikacji przed krytycznym 13. miesiącem życia klienta, w którym dane wykazują największy "Churn spike".
2.  **Program VIP:** Wprowadzenie dedykowanego opiekuna dla najbardziej dochodowego segmentu.
3.  **Konwersja One-Timers:** Program lojalnościowy po pierwszym zakupie, mający na celu przesunięcie klientów "weryfikujących produkt" do grupy Loyal.
4.  **Uproszczenie odnowień rocznych:** Gwałtowny spadek na krzywej przetrwania (Survival Curve) po roku sugeruje potrzebę uproszczenia procesu płatności lub dodania zachęt finansowych przy przedłużeniu.

---

##  Wnioski z analizy promocji
*   **Efekt ostatniej szansy:** Klienci najczęściej kupują w ostatnim dniu promocji – kluczowe jest stosowanie przypominajek „Last call”.
*   **Wzrost cen:** Informacja o planowanej podwyżce w marcu zadziałała motywująco, zwiększając sprzedaż droższych pakietów.
*   **Jakość vs Ilość:** Agresywne promocje (Black Week) zwiększają liczbę nowych klientów, ale często obniżają średnią jakość kohorty (wyższa podatność na churn).

---
*Analiza wykonana przez Agnieszkę Trzciałkowską | Maj 2026 | Dane: KajoData*
