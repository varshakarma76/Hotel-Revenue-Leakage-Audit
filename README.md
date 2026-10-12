# Hotel Revenue Leakage Audit

**Tools:** Python (Pandas, NumPy, Matplotlib, Seaborn) | Power BI ,DAX

An Exploratory Data Analysis (EDA) and dashboard project that finds why hotel bookings get cancelled and estimates how much revenue is lost because of those cancellations.

---

## Problem Statement

Hotel rooms are perishable inventory. When a booking is cancelled and the room stays empty, that revenue is lost. This project aims to:

- Identify the main factors linked to booking cancellations
- Detect high-risk booking profiles
- Compare revenue patterns across distribution channels
- Estimate the historical revenue lost due to cancellations

---

## Dataset

- **Source:** Kaggle - (https://www.kaggle.com/datasets/jessemostipak/hotel-booking-demand)
- **Size:** 119,390 bookings, 32 columns (City Hotel and Resort Hotel, 2015-2017)
- **Target variable:** `is_canceled` (0 = not cancelled, 1 = cancelled)

---

## Project Workflow

**Phase 0: Data overview**
- Checked shape, data types, missing values, and duplicates
- Filled missing `children` values with 0
- Found 31,994 duplicate rows. They were **kept**, because the dataset has no unique booking ID and they may be genuine bookings

**Phase 1: Data integrity audit**
- **Ghost guest bookings:** 180 bookings (0.15%) with zero adults, children, and babies
- **Financial anomalies:** 1,747 non-cancelled bookings with ADR <= 0 (1,746 with ADR = 0, 1 negative)
- Combined the arrival year, month, and day columns into one `arrival_date` column

**Phase 2: Cancellation drivers**
- Deposit type, lead time, and customer type analysis
- Found a data-driven **lead-time risk cut-off** using a rolling cancellation rate

**Phase 3: Channel and room analysis**
- Distribution channel vs ADR vs cancellation rate
- Reserved vs assigned room type comparison

**Phase 4: Revenue leakage**
- Estimated lost revenue = `ADR x (weekend nights + week nights)` for cancelled bookings
- Broken down by hotel, market segment, and hotel x segment

---

## Key Findings

| Area | Finding |
|---|---|
| Overall cancellation rate | **37.04%** (about 1 in 3 bookings) |
| Estimated lost revenue | **$16.73M** (City Hotel ~$10.9M, Resort Hotel ~$5.8M) |
| Biggest source of loss | **Online TA** with ~$10.2M (~61% of total loss) |
| Lead-time risk cut-off | **188+ days** has a **57.52%** cancellation rate vs **32.11%** below 188 days |
| Customer type | Transient 40.75% > Contract 30.96% > Transient-Party 25.43% > Group 10.23% |
| Distribution channel | TA/TO 41.03% vs Direct 17.46% cancellation rate |
| Data quality | 180 zero-guest bookings, 1,747 zero/negative ADR stays |

**Notes on the findings**
- **Non-Refund deposits** show a 99.36% cancellation rate, which is unexpected. It is flagged for further investigation (it could be policy behaviour or a data recording issue).
- Bookings with a **room change** show a much lower cancellation rate (5.38%). This does not mean room changes reduce cancellations. Rooms are usually reassigned at check-in, so cancelled bookings rarely reach that stage.
- Zero-ADR bookings are expected for complimentary stays, but zero-ADR bookings in paid channels (Online TA, Offline TA/TO) should be audited.

---

## Power BI Dashboard

The dashboard has **2 pages**:

1. **Hotel Booking Executive Overview:** KPIs (total bookings, cancellation rate, estimated lost revenue, average ADR, zero/negative ADR bookings), cancellation trend by month, cancellation by channel and lead-time group, lost revenue by hotel
2. **Revenue Leakage & Risk Analysis:** ghost bookings, lost revenue by market segment, cancellation by customer type, lead-time risk group, deposit type, key insights and recommendations

Filters: hotel, year, month, distribution channel



## Recommendations

- Use **188+ days of lead time** as an early-warning risk threshold
- Monitor **TA/TO and Online TA** channels closely and consider stronger cancellation controls
- Prioritize **City Hotel** for revenue-recovery actions
- Review Non-Refund booking and cancellation patterns
- Validate **ADR <= 0** records before financial reporting
- Add system validation to stop bookings with zero guests

---

## Limitations

- Lost revenue is an **estimate** (ADR x nights), not actual accounting data
- No unique booking ID, so exact duplicates could not be confirmed as errors
- Historical data (2015-2017), so patterns may differ today

---

## Repository Structure

```
├── hotel_booking.ipynb              # EDA notebook
├── hotel_booking_dashboard.pbix     # Power BI dashboard
├── hotel_bookings.csv               # Dataset (or link to Kaggle if large)
├── Hotel Revenue leakage_overview.png |  Hotel_Revenue leakage_analysis.png
└── README.md
```

---

## How to Run

1. Clone the repository
2. Install the libraries: `pip install pandas numpy matplotlib seaborn`
3. Open `hotel_booking.ipynb` in Jupyter Notebook and run all cells
4. Open the `.pbix` file in Power BI Desktop to explore the dashboard

---

## Author

**Varsha Karma**
[LinkedIn](https://www.linkedin.com/in/varsha-karma) | [GitHub](https://github.com/varshakarma76)
