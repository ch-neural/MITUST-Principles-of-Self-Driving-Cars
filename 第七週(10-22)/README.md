# 第七週（10-22）第三章 感測器（三）：失效條件與感測器融合

三節課。第 1 節：每一種感測器什麼時候會壞、壞了會不會說；第 2 節：既然每一顆都會壞，怎麼合起來；
第 3 節：學生自己打開 nuScenes 的真實資料，同一個瞬間看相機、光達、雷達各看到什麼，再把三種感測器拼成一句話。
大綱原本寫的「用手機拍相機失效的畫面」沒有採用（2026-10-15 老師決定）；融合只講觀念和例子，不出卡爾曼濾波的公式。

## 學生用的線上工具

- nuScenes 觀察台：**https://ch-neural.github.io/MITUST-Principles-of-Self-Driving-Cars/week7/nusc/**
- 作業頁：**https://ch-neural.github.io/MITUST-Principles-of-Self-Driving-Cars/week7/hw/**

兩頁由 `.github/workflows/pages.yml` 自動發布（`第七週_nuScenes觀察台.html → week7/nusc/`、`第七週_作業.html → week7/hw/`）。
網址與 QR 印在第 3 節第 2 張。兩頁互相連結：在網站上連 `../hw/`、`../nusc/`；直接開 html 檔時改連同一個資料夾裡的另一個檔案。

**觀察台**：nuScenes mini 的四個關鍵畫面（波士頓白天、新加坡停車場、新加坡夜間、新加坡夜間雨後）。
前方相機照片上可以開關光達點、雷達點、物體框、「合起來」（框上寫「是什麼・多遠・多快」）；下面是鳥瞰圖；右邊是點數和每個物體的表格：
距離、光達點、雷達點（官方標註的 `num_radar_pts`）、雷達量到的速度（朝本車靠近或遠離的那一段，只用畫出來的、過濾後的點算）、
在不在前方相機畫面裡（不在的寫左側、右側、後方）。照片、點雲、框都內嵌在單一 HTML 裡，離線可用。

**作業頁**：七個任務 22 題——四個場景各一個任務、任務五搭檔卡（把相機、光達、雷達說的拼成一句話；含「橫著開的車雷達量到 ≈0」）、
任務六把第三週那張「誰來補」的表再填一次（加兩列今天看到的）、任務七選一句結論。和前兩週一樣，答案存在裝置上，按「產生作答單」截圖交 LINE。
所有數字題的答案都從觀察台的資料重算過；解答不嵌進學生頁，也不提交到 repo。

## 檔案

| 檔案 | 用途 |
|---|---|
| `第七週_第1節_簡報.pptx` | 17 張／53 分。前四週每週一句結論（第三到第六週）、三種壞法（看不到、看錯、看不到卻說沒事）、失效對照表、雨霧（兩張實拍照片＋兩支影片）、夜間與雷達的 nuScenes 真實畫面、雷達的點大多不在車上（鳥瞰圖）、十段場景的統計、鏡面與多路徑、髒污（影片）、IMU 漂移、雷達看人：看得到但抓不穩（鳥瞰圖＋光達）、新一代雷達怎麼看人、看不到卻說沒事 |
| `第七週_第2節_簡報.pptx` | 12 張／55 分。兩顆說的不一樣（自製動圖：頭頂路牌和路牌下的車，雷達報出同一個點）、互補與備援、標定（光達故意轉偏 4 度）、時間同步（光達早 0.2 秒，橫越的車點留在原地）、前期／中期／後期融合、對應（影片）、聽誰的、定位融合、降級、五組常見的搭檔 |
| `第七週_第3節_簡報.pptx` | 12 張／58 分。開兩個網頁、認識 nuScenes、觀察台怎麼用、任務一到六的提問（答案在備忘稿；任務七的結論句在作業頁）、新一代雷達補上高度、小結與下週預告 |
| `第七週_nuScenes觀察台.html` | 課堂工具，單檔，約 950 KB |
| `第七週_作業.html` | 作業頁，單檔，約 23 KB |

**老師的講稿全部放在簡報的備忘稿裡**。三節都用 `tools_notes_fit.py` 比對過，每一張都印得出來；投影片上的頁碼交叉引用用 `tools_xref.js` 檢查過。

## 資料與圖片來源

- **nuScenes**（Motional），v1.0-mini，授權 CC BY-NC-SA 4.0（非商業使用、註明出處、同樣方式分享）。雷達是 Continental ARS 408-21，點照官方工具的預設方式過濾；光達是 Velodyne HDL-32E。投影、數點、速度與鳥瞰圖由課程自己寫的程式完成，不用官方套件。投影片上每一張都標了場景編號。
- 第 1 節第 6 張照片取自 Wikimedia Commons：左 `Rain on windshield (52352547682).jpg`（NPS／Joshua Tree National Park，公有領域）；右 `Road in autumn fog, Germany (Marburg) 2016-10-27.jpg`（Ludwig Sebastian Micheler，CC BY-SA 4.0）。
- 第 1 節第 11 張動圖取自第五週（課程自製），第 13 張漂移曲線取自第六週。第 2 節第 2 張動圖為課程自製（高度放大畫）。
- 「人的雷達反射比車弱」：E. Schubert et al., *Human RCS Measurements and Dummy Requirements for the Assessment of Radar Based Active Pedestrian Safety Systems*, IRS 2013。新一代 4D 雷達看行人的成績：View-of-Delft 資料集（Palffy et al., IEEE RA-L 2022）與 RadarPillars（arXiv 2408.05020）。
- 影片（線上連結，播放需要網路）：Mark Rober `IQJL3htsDyQ`、工研院 `zTLa0CFpyXY`、Valeo `vhjFmzcD-iQ`（第 1 節）；NVIDIA DRIVE Labs `cMlGyIJH5L8`（第 2 節）。都下載看過畫面，口白裡的時間點是從畫面或旁白轉寫讀的。
