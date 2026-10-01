# Domain & Evaluation Change Summary

## Mục đích thay đổi

Hoàn thiện domain OrbitTech Customer Support để có benchmark tái lập được: một golden dataset có evidence truy xuất được, câu trả lời thực tế từ RAG assistant, và báo cáo evaluation/failure analysis dựa trên artifact thay vì nhận xét thủ công.

## Trước thay đổi

| Hạng mục | Trạng thái |
|---|---|
| Golden dataset | 20 khung QA rỗng, chưa có câu hỏi, expected answer hay evidence. |
| Domain run | Chưa có `actual_answers.json`; chưa đánh giá được chất lượng RAG thực tế. |
| Evaluation report | Exercise 3.1–3.3 và reflection chưa có kết quả, metric hoặc phân tích failure. |
| Quality check | Không có kết quả validator cho dataset đã hoàn thiện. |

## Sau thay đổi

| Hạng mục | Kết quả |
|---|---|
| Golden dataset | 20/20 QA: 5 easy, 7 medium, 5 hard, 3 adversarial; phủ 10/10 tài liệu; validator PASS. |
| Domain run | `artifacts/actual_answers.json` lưu 20 câu trả lời và top-5 retrieved chunks/câu. |
| Evaluation report | `artifacts/benchmark_results.json` lưu per-case metrics, aggregate và improvement log. |
| Benchmark | 17/20 pass (85.0%); Recall .878, Precision .948, Faithfulness .705, Relevance .742, Completeness .711. |
| Documentation | `exercises.md` có Exercise 3.1–3.3; `reflection.md` có failure analysis, regression strategy và improvement loop. |
| Verification | `pytest tests -q`: 42 passed; `validate_golden_dataset.py`: PASS. |

## Diễn giải kết quả

Retrieval đã tốt vì Context Recall và Context Precision đều cao. Điểm cần cải thiện nằm ở generation/guardrail: A01 chưa redirect đúng về phạm vi OrbitTech, A03 không nêu giới hạn không thể thao tác live order, và A02 cần redirect sau khi từ chối prompt injection. Vì vậy, ưu tiên thay đổi tiếp theo là scope/capability template, kiểm tra claim-to-context và regression gate riêng cho safety.
