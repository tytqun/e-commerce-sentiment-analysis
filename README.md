# Udemy AI Course Reviews – EDA

## 📦 Dữ liệu nguồn
- 22 CSV chuẩn hoá `udemy_data_*.csv` (review text, rating, thời gian tương đối, metadata URL).
- `general_course_data.xlsx` (title, badge, skill_level, lecture_count, content_length, price gốc/giảm, num_students).
- Notebook chính: `analysis/udemy_eda.ipynb` (pipeline cơ bản) và `analysis/udemy_eda_complete.ipynb` (phiên bản đầy đủ với viz/bảng mở rộng).

## 🧹 Tiền xử lý chính
- Gộp toàn bộ CSV, xoá 194 review trùng (trùng slug + reviewer + text).
- Chuẩn hoá rating & thang điểm, tạo `normalized_rating`.
- Chuyển `review_time_relative` → `weeks_ago`, cờ `recent_flag` (≤4 tuần).
- Tokenize, đếm từ sentiment hoặc neutral.
- Parse metadata: nội suy `content_hours`, `lecture_count`, `num_students` theo median skill-level; tính `discount_pct`, `price_per_hour`, `students_per_hour`.
- Phát hiện ngôn ngữ (`langdetect`) + sentiment transformer cho review tiếng Anh.

## 🧠 Feature engineering nổi bật
- `reviews_per_student`, `discount_price_per_student`, `reviews_per_hour`, `recent_reviews_density`.
- `recent_reviews_est = review_count × recent_share`; `trend_per_review`; `rating_vs_catalog`; `rating_to_price_ratio`.
- Momentum 4-week (`recent_4w_mean`, `prior_4w_mean`, `momentum_delta`, `momentum_ratio`).
- Ngôn ngữ: `english_share`, `transformer_positive_share`, `positive_minus_negative_share`, `english_review_gap`.

## 📊 Biểu đồ chính (lưu tại `outputs/figures/`)
- `rating_distribution.png`
- `weekly_review_trend.png` (kèm rolling mean 4 tuần)
- `ratings_by_badge_skill.png`
- `price_vs_rating.png`, `price_vs_students.png`
- `reviews_per_student_distribution.png`, `reviews_per_student_vs_rating.png`
- `course_feature_correlation.png`

## 📑 Bảng output quan trọng (trong `outputs/tables/`)
- `course_summary.csv`: bảng tổng hợp khóa (rating, sentiment, giá, momentum, feature mới).
- `reviews_per_course.csv`, `course_weekly_reviews.csv`, `course_momentum_summary.csv`.
- `language_distribution.csv`, `language_mix_by_course.csv`.
- `top_engagement_courses.csv`, `momentum_leaders.csv`, `value_courses.csv`.
- `keyword_summary.csv` + `keyword_summary_raw.csv`.
- `top_courses.csv`, `bottom_courses.csv` (đã thêm density & rating comparison).

## 🧾 Snapshot kết quả (từ notebook)
- 15,971 review hợp lệ sau loại trùng.
- 22 khóa, 12,971 reviewer, median review age ≈ 26 tuần.
- 78.7% review tiếng Anh; transformer sentiment: 67.0% positive, 11.6% negative, 21.3% không đánh giá.
- Momentum tăng mạnh: “Generative AI 2025 Executive Briefing”, “Complete Agentic AI Engineering Course”, “Product Management for AI & Data Science”.
- Tương quan giá-rating yếu (corr ≈ -0.145), giá-số học viên hơi dương (≈ 0.194).

## 🚀 Đề xuất tiếp tục
1. Triển khai sentiment/topic cho review đa ngôn ngữ (dịch hoặc dùng mô hình multilingual).
2. Kết hợp momentum với `reviews_per_student` để ưu tiên hỗ trợ khóa tụt hạng nhưng có học viên lớn.
3. Tự động hóa notebook (Papermill/Airflow) khi có CSV mới.
4. Dashboard hóa (Power BI / Looker) dùng `course_summary.csv` & các figure nguồn.

## 🛠 Chạy dự án
```bash
# Tạo môi trường và cài thư viện
python -m venv .venv
.\.venv\Scripts\activate
pip install -r requirements.txt

# Chạy notebook đầy đủ (hoặc mở trong JupyterLab)
jupyter nbconvert --to notebook --execute analysis/udemy_eda_complete.ipynb --inplace
```

@2025 – Udemy AI Course Reviews Analytics
# e-commerce-sentiment-analysis
