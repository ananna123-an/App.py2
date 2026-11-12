import streamlit as st
import pandas as pd
import plotly.express as px
import yfinance as yf
from fredapi import Fred

# 頁面基本配置
st.set_page_config(page_title="總體經濟與金融即時儀表板", layout="wide")
st.title("🌐 全球總體經濟與金融指標即時儀表板")

# 側邊欄：輸入 API Key 與刷新控制
st.sidebar.header("⚙️ 設定與金鑰")

# 優先使用 Streamlit Secrets，若無則讓學生在側邊欄輸入
if "FRED_API_KEY" in st.secrets:
    fred_key = st.secrets["FRED_API_KEY"]
    st.sidebar.success("已自動讀取 Secrets 中的 API Key！")
else:
    fred_key = st.sidebar.text_input("請輸入你的 FRED API Key:", type="password")

if st.sidebar.button("手動刷新數據"):
    st.cache_data.clear()

# 1. 抓取 FRED 數據 (設快取 1 小時)
@st.cache_data(ttl=3600)
def get_fred_data(api_key):
    fred = Fred(api_key=api_key)
    data = {
        "CPI 物價指數": fred.get_series("CPIAUCSL"),
        "10年期美債殖利率": fred.get_series("DGS10"),
        "2年期美債殖利率": fred.get_series("DGS2"),
        "美國失業率": fred.get_series("UNRATE"),
        "聯邦基金利率": fred.get_series("FEDFUNDS")
    }
    df = pd.DataFrame(data).ffill().tail(365)
    
    # 計算 10年期減去 2年期美債利差
    df["10年-2年美債利差"] = df["10年期美債殖利率"] - df["2年期美債殖利率"]
    return df

# 2. 抓取 Market 數據 (設快取 10 分鐘)
@st.cache_data(ttl=600)
def get_market_data():
    tickers = {
        "S&P 500": "^GSPC",
        "台灣加權指數": "^TWII",
        "布蘭特原油": "BZ=F",
        "美元/台幣匯率": "TWD=X"
    }
    df = yf.download(list(tickers.values()), period="1y")["Close"]
    df.rename(columns={v: k for k, v in tickers.items()}, inplace=True)
    return df

# 頁面渲染邏輯
if fred_key:
    try:
        macro_df = get_fred_data(fred_key)
        market_df = get_market_data()

        # 第一區塊：核心 KPI 卡片 (擴充為 5 欄以容納利差)
        st.subheader("📊 關鍵總體經濟指標 (FRED)")
        col1, col2, col3, col4, col5 = st.columns(5)
        
        latest_cpi = macro_df["CPI 物價指數"].dropna().iloc[-1]
        col1.metric("CPI 物價指數", f"{latest_cpi:.1f}")
        
        latest_rate = macro_df["10年期美債殖利率"].dropna().iloc[-1]
        rate_prev = macro_df["10年期美債殖利率"].dropna().iloc[-2]
        col2.metric("10年美債殖利率", f"{latest_rate:.2f}%", f"{latest_rate - rate_prev:+.2f}%")
        
        latest_spread = macro_df["10年-2年美債利差"].dropna().iloc[-1]
        spread_prev = macro_df["10年-2年美債利差"].dropna().iloc[-2]
        col3.metric("10Y-2Y 美債利差", f"{latest_spread:.2f}%", f"{latest_spread - spread_prev:+.2f}%")
        
        latest_unemp = macro_df["美國失業率"].dropna().iloc[-1]
        col4.metric("美國失業率", f"{latest_unemp:.1f}%")
        
        latest_fed = macro_df["聯邦基金利率"].dropna().iloc[-1]
        col5.metric("聯邦基金利率", f"{latest_fed:.2f}%")

        # 殖利率曲線倒掛警示邏輯
        st.write("---")
        if latest_spread < 0:
            st.error(f"🚨 **市場警示：美國公債殖利率曲線倒掛！** 當前 10年期與 2年期公債利差為 **{latest_spread:.2f}%**（低於 0）。歷史經驗顯示，倒掛通常是經濟衰退的重要領先指標，請密切留意市場風險。")
        else:
            st.success(f"✅ **殖利率曲線正常**：當前 10年期與 2年期公債利差為 **{latest_spread:.2f}%**（高於 0）。")

        # 第二區塊：互動式圖表
        st.write("---")
        st.subheader("📈 市場走勢與總體指標折線圖")
        
        tab1, tab2, tab3 = st.tabs(["金融市場走勢 (yfinance)", "總體經濟趨勢 (FRED)", "📉 10Y-2Y 美債利差專區"])
        
        with tab1:
            market_choice = st.selectbox("選擇金融市場標的：", market_df.columns, key="market_select")
            fig1 = px.line(market_df, y=market_choice, title=f"{market_choice} 近一年走勢")
            st.plotly_chart(fig1, use_container_width=True)
            
        with tab2:
            macro_choice = st.selectbox("選擇總體經濟指標：", macro_df.columns, key="macro_select")
            fig2 = px.line(macro_df, y=macro_choice, title=f"{macro_choice} 近一年走勢")
            st.plotly_chart(fig2, use_container_width=True)

        with tab3:
            st.markdown("### 美國 10 年期與 2 年期公債殖利率利差歷史走勢")
            st.markdown("當利差小於 0（即綠色零軸下方時）代表發生**殖利率曲線倒掛**。")
            
            # 繪製利差折線圖，並加入 0 軸參考線方便觀察倒掛情形
            fig_spread = px.line(macro_df, y="10年-2年美債利差", title="10年期 - 2年期美債殖利率利差 (近一年)")
            fig_spread.add_hline(y=0, line_dash="dash", line_color="red", annotation_text="零軸 (倒掛分界線)", annotation_position="bottom right")
            st.plotly_chart(fig_spread, use_container_width=True)

    except Exception as e:
        st.error(f"數據讀取失敗，請確認 FRED API Key 是否正確。錯誤訊息: {e}")
else:
    st.info("👈 請在左側輸入您的 FRED API Key 以開啟儀表板。")
