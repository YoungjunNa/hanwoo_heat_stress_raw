# hanwoo_heat_stress_raw

[![DOI](https://zenodo.org/badge/1251056677.svg)](https://doi.org/10.5281/zenodo.22283542)

한우 거세우 열스트레스 분석 원시 데이터 / Raw data for heat stress analysis in Hanwoo steers

---

## 개요 / Overview

**한국어**
본 데이터셋은 한우 거세우의 열스트레스(Heat Stress) 노출 기간과 도체성적(carcass traits)의 관계를 분석하기 위해 구축된 원시 데이터입니다. 개체 식별정보(cattleNo)는 SHA-256 해시 처리를 통해 익명화되었습니다.

**English**
This dataset was constructed to analyze the relationship between heat stress exposure duration and carcass traits in Hanwoo steers. Individual identification numbers (cattleNo) have been anonymized using SHA-256 hashing.

---

## 데이터 설명 / Data Description

| 변수 / Variable | 설명 / Description |
|---|---|
| `cattleNo` | 개체번호 (SHA-256 해시 익명화) / Animal ID (SHA-256 hashed) |
| `지점` | 기상청 관측소 코드 / KMA station code |
| `지점명` | 기상청 관측소명 / KMA station name |
| `일시` | 관측 일시 / Observation date |
| `평균기온_c` | 일평균기온 (°C) / Daily mean temperature (°C) |
| `평균_상대습도_percent` | 일평균 상대습도 (%) / Daily mean relative humidity (%) |
| `일강수량_mm` | 일강수량 (mm) / Daily precipitation (mm) |
| `평균_풍속_m_s` | 일평균 풍속 (m/s) / Daily mean wind speed (m/s) |
| `year` | 연도 / Year |
| `thi` | 온습도지수 / Temperature-Humidity Index (THI) |
| `HS` | 열스트레스 여부 (1 = HS, 0 = non-HS) / Heat stress indicator |
| `CS` | 한냉스트레스 여부 (1 = CS, 0 = non-CS) / Cold stress indicator |

---

## 익명화 방법 / Anonymization

개체번호(`cattleNo`)는 `digest` 패키지의 SHA-256 알고리즘을 사용하여 해시 처리되었습니다. 동일 개체는 데이터셋 전반에 걸쳐 동일한 해시값을 가집니다.

Individual IDs (`cattleNo`) were hashed using the SHA-256 algorithm via the R `digest` package. The same individual is consistently represented by the same hash value across the dataset.

```r
library(digest)
digest("002164985512", algo = "sha256") %>% substr(1, 8)
```

---

## 데이터 불러오기 / How to Load

```r
# GitHub에서 직접 불러오기 / Load directly from GitHub
url <- "https://github.com/YoungjunNa/hanwoo_heat_stress_raw/raw/main/hanwoo_HS_result.rds"
temp <- tempfile(fileext = ".rds")
download.file(url, destfile = temp, mode = "wb")
result <- readRDS(temp)

# 로컬 파일로 저장 후 불러오기 / Load from local file
saveRDS(result, "hanwoo_HS_result.rds")
result <- readRDS("hanwoo_HS_result.rds")
```

---

## 데이터 구조 / Data Structure

```r
# list 구조 확인
length(result)       # 개체 수 / Number of animals
names(result)        # 인덱스 확인 / Check indices

# 첫 번째 개체 확인
result[[1]]

# 전체 데이터 병합
library(dplyr)
result_bind <- bind_rows(result)
```

---

## 열스트레스 기준 / Heat Stress Threshold

| 구분 / Category | 기준 / Threshold |
|---|---|
| 열스트레스 (HS) | THI ≥ 72 |
| 한냉스트레스 (CS) | 기온 < −6.7°C / Temperature < −6.7°C |

---

## 기상 데이터 출처 / Weather Data Source

기상청 종관기상관측(ASOS) / Korea Meteorological Administration (KMA) Automated Synoptic Observing System (ASOS)

---

## 라이선스 / License

본 데이터는 연구 목적으로만 사용 가능합니다. / This dataset is available for research purposes only.
