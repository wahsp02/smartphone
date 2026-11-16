# pc_waveform_monitor.py - 電力品質多維度互動式網頁分析儀
import streamlit as st
import pandas as pd
import plotly.graph_objects as go
import os

# 頁面基本設定 (寬螢幕模式，最適合工程圖表展現)
st.set_page_config(
    page_title="電力品質多維度互動分析儀",
    page_icon="⚡",
    layout="wide",
    initial_sidebar_state="expanded"
)

FILE_PATH = "20261114.xlsx"

st.title("⚡ 電力系統實測數據與多維度波形互動分析儀")
st.markdown("本系統直接讀取老師提供的實測電力資料檔 (`20261114.xlsx`)，提供動態時間軸過濾、多相功率對比與諧波失真分析。")

# 檢查檔案是否存在
if not os.path.exists(FILE_PATH):
    st.error(f"❌ 找不到檔案 `{FILE_PATH}`！請確認它與這支 Python 程式放在同一個資料夾內。")
else:
    # 讀取與清洗 Excel 數據 (快取以加速效能)
    @st.cache_data
    def load_power_data():
        df = pd.read_excel(FILE_PATH, sheet_name="sheet1", engine="openpyxl")
        # 修正第 2 筆之後 Time 歸零的問題：以第一筆時間為起點，每秒遞增重建完整時間軸
        start_time = pd.to_datetime(df["Time"].iloc[0])
        df["Parsed_Time"] = [start_time + pd.Timedelta(seconds=i) for i in range(len(df))]
        df["Time_Str"] = df["Parsed_Time"].dt.strftime("%H:%M:%S")
        return df

    df = load_power_data()

    # --- 側邊欄：互動控制面板 ---
    with st.sidebar:
        st.header("🎛️ 互動控制面板")
        st.markdown("在這裡可以自由調整觀測的時間範圍與圖表顯示項目：")
        
        # 1. 時間範圍選擇器 (Slider)
        max_idx = len(df) - 1
        time_range = st.slider(
            "選擇觀測資料索引範圍 (Time Window)",
            0, max_idx, (0, max_idx)
        )
        
        st.markdown("---")
        st.subheader("📊 圖表顯示開關")
        show_total_power = st.checkbox("顯示總功率 (P / AP / Q)", value=True)
        show_phase_power = st.checkbox("顯示 A/B 雙相實功率對比", value=True)
        show_thd = st.checkbox("顯示電流與中性線諧波失真 (THD)", value=True)
        show_pf = st.checkbox("顯示功率因數 (pf)", value=True)

    # 根據側邊欄選擇的時間範圍過濾資料
    df_filtered = df.iloc[time_range[0]:time_range[1]+1]

    # --- 頂部即時統計摘要 (KPI Cards) ---
    st.subheader("📌 當前選定區間之統計摘要")
    k1, k2, k3, k4 = st.columns(4)
    k1.metric("平均總實功率", f"{df_filtered['Total P (W)'].mean():.1f} W")
    k2.metric("區間最高實功率", f"{df_filtered['Total P (W)'].max():.1f} W")
    k3.metric("平均功率因數", f"{df_filtered['Total pf'].mean():.4f}")
    k4.metric("最高中性線諧波 I_THD_N", f"{df_filtered['I_THD_N (%)'].max():.1f} %")

    st.markdown("---")

    # --- 互動圖表展示區 ---
    if show_total_power:
        st.subheader("1️⃣ 總電力參數時域波形 (Total P, AP, Q)")
        fig_p = go.Figure()
        fig_p.add_trace(go.Scatter(x=df_filtered["Time_Str"], y=df_filtered["Total AP (VA)"], mode="lines", name="總視在功率 AP (VA)", line=dict(color="#3A86FF", width=2)))
        fig_p.add_trace(go.Scatter(x=df_filtered["Time_Str"], y=df_filtered["Total P (W)"], mode="lines", name="總實功率 P (W)", line=dict(color="#FF006E", width=2, dash="dash")))
        fig_p.add_trace(go.Scatter(x=df_filtered["Time_Str"], y=df_filtered["Total Q (Var)"], mode="lines", name="總虛功率 Q (Var)", yaxis="y2", line=dict(color="#FFBE0B", width=2)))
        fig_p.update_layout(
            height=350, margin=dict(l=20, r=20, t=30, b=20),
            yaxis=dict(title="功率 (W / VA)"),
            yaxis2=dict(title="虛功率 Q (Var)", overlaying="y", side="right"),
            legend=dict(orientation="h", y=1.12)
        )
        st.plotly_chart(fig_p, use_container_width=True)

    if show_phase_power:
        st.subheader("2️⃣ A 相與 B 相實功率比較 (P_A vs P_B)")
        fig_ab = go.Figure()
        fig_ab.add_trace(go.Scatter(x=df_filtered["Time_Str"], y=df_filtered["P_A (W)"], mode="lines", name="A相實功率 P_A (W)", line=dict(color="#00B4D8", width=2)))
        fig_ab.add_trace(go.Scatter(x=df_filtered["Time_Str"], y=df_filtered["P_B (W)"], mode="lines", name="B相實功率 P_B (W)", line=dict(color="#8338EC", width=2)))
        fig_ab.update_layout(
            height=350, margin=dict(l=20, r=20, t=30, b=20),
            yaxis=dict(title="實功率 (Watt)"),
            legend=dict(orientation="h", y=1.12)
        )
        st.plotly_chart(fig_ab, use_container_width=True)

    if show_thd:
        st.subheader("3️⃣ 電流與中性線諧波失真率分析 (THD %)")
        fig_thd = go.Figure()
        fig_thd.add_trace(go.Scatter(x=df_filtered["Time_Str"], y=df_filtered["I_THD_A (%)"], mode="lines", name="A相電流諧波 I_THD_A (%)", line=dict(color="#FB5607", width=2)))
        fig_thd.add_trace(go.Scatter(x=df_filtered["Time_Str"], y=df_filtered["I_THD_B (%)"], mode="lines", name="B相電流諧波 I_THD_B (%)", line=dict(color="#2EC4B6", width=2)))
        fig_thd.add_trace(go.Scatter(x=df_filtered["Time_Str"], y=df_filtered["I_THD_N (%)"], mode="lines", name="中性線諧波 I_THD_N (%)", line=dict(color="#E63946", width=2, dash="dot")))
        fig_thd.update_layout(
            height=350, margin=dict(l=20, r=20, t=30, b=20),
            yaxis=dict(title="諧波失真率 (%)"),
            legend=dict(orientation="h", y=1.12)
        )
        st.plotly_chart(fig_thd, use_container_width=True)

    if show_pf:
        st.subheader("4️⃣ 功率因數分析 (Total pf & A/B 相 pf)")
        fig_pf = go.Figure()
        fig_pf.add_trace(go.Scatter(x=df_filtered["Time_Str"], y=df_filtered["Total pf"], mode="lines", name="總功率因數 Total pf", line=dict(color="#3A86FF", width=2)))
        fig_pf.add_trace(go.Scatter(x=df_filtered["Time_Str"], y=df_filtered["pf_A"], mode="lines", name="A相功率因數 pf_A", line=dict(color="#FF006E", width=1.5, dash="dash")))
        fig_pf.add_trace(go.Scatter(x=df_filtered["Time_Str"], y=df_filtered["pf_B"], mode="lines", name="B相功率因數 pf_B", line=dict(color="#2EC4B6", width=1.5, dash="dash")))
        fig_pf.update_layout(
            height=350, margin=dict(l=20, r=20, t=30, b=20),
            yaxis=dict(title="功率因數 (pf)", range=[0.95, 1.01]),
            legend=dict(orientation="h", y=1.12)
        )
        st.plotly_chart(fig_pf, use_container_width=True)

    # --- 底部：原始資料檢視與匯出 ---
    with st.expander("📄 檢視與下載目前選定區間的詳細數據表格"):
        st.dataframe(df_filtered.drop(columns=["Parsed_Time"]), use_container_width=True)
        
        # 一鍵下載按鈕
        csv_bytes = df_filtered.drop(columns=["Parsed_Time"]).to_csv(index=False).encode("utf-8-sig")
        st.download_button(
            label="📥 下載過濾後的資料為 CSV 報表",
            data=csv_bytes,
            file_name="filtered_power_quality_report.csv",
            mime="text/csv"
        )
