# Performance Analysis
Front-end performance analysis with Sitespeed.io.

### Connectivity profiles (Sitespeed/Throttle reference)

The table below lists pre-made profiles from Sitespeed Throttle. Bandwidth values are in **kbit/s** (upload and download) and RTT is in **ms**.

| Profile  | Upload (kbit/s) | Download (kbit/s) | RTT (ms) | Approx. Download (Mbps) |
|---------:|----------------:|------------------:|---------:|------------------------:|
| **4g**   | 9000            | 9000              | 85       | ~9.00 Mbps              |
| **3g**   | 768             | 1600              | 150      | ~1.60 Mbps              |
| **3gfast** | 768           | 1600              | 75       | ~1.60 Mbps              |
| **3gslow** | 400           | 400               | 200      | ~0.40 Mbps              |
| **3gem** | 400             | 400               | 200      | ~0.40 Mbps              |
| **2g**   | 256             | 280               | 400      | ~0.28 Mbps              |
| **cable**| 1000            | 5000              | 14       | ~5.00 Mbps              |
| **lte**  | 12000           | 12000             | 35       | ~12.00 Mbps             |

Source: [Sitespeed Throttle documentation](https://www.sitespeed.io/documentation/throttle)

**Tip:** If you need to override these defaults, create a `custom` profile with `--connectivity.down`, `--connectivity.up`, and `--connectivity.rtt`, or configure the `throttle` service manually.

### Peru mobile baseline (July 2025)

National 4G indicators:

| Metric | Value |
|---|---:|
| Average 4G download speed | 12.16 Mbps |
| Average 4G upload speed | 2.96 Mbps |
| Average latency | 54.10 ms |
| 4G coverage time | 92.65% |
| Packet loss | 0.79% |

Average 4G download speed by operator (national):

| Operator | Avg download (Mbps) |
|---|---:|
| Claro | 12.65 |
| Bitel | 12.50 |
| Entel | 11.90 |
| Movistar | 11.09 |

Source: [OSIPTEL press release, 29 Aug 2025](https://www.gob.pe/institucion/osiptel/noticias/1235780-osiptel-estos-fueron-los-indicadores-de-calidad-de-internet-movil-en-julio-de-2025)

![4G coverage time in Peru - July 2025](assets/tiempo-cobertura-julio-2025.png)

*Source: Panel de Monitoreo de Internet Móvil, consolidated report for July 2025 (page 15).* [PDF](https://sociedadtelecom.pe/wp-content/uploads/2025/08/Panel_de_Monitoreo_Internet_Movil_Consolidado_Julio_2025.pdf)

### Running a test

Requirements:

- Docker Desktop (or Docker Engine) installed and running.

1. Install the dependencies (one-time setup):
   ```bash
   npm install
   ```
2. Add URLs to `urls.txt` (one per line). You can optionally add an alias after the URL:
   ```txt
   https://acme.com home_prod
   http://qa.google.com/ qa_google
   ```
3. Run tests with one of the project scripts:
   ```bash
   npm run perf:4g
   npm run perf:3g
   ```
4. Review the HTML report output under `./sitespeed-result/` (a timestamped folder is created per run).

**Important:** The `throttle` connectivity engine tweaks your system network rules to emulate slow links and those changes persist until you remove them. After you finish your tests, run `sudo throttle --stop` to restore your normal bandwidth (otherwise all your apps will keep using the throttled profile). You can check whether throttling is still active with `sudo throttle --status`.
