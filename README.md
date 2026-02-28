import streamlit as st
import pandas as pd
import plotly.express as px

# Page config for wide layout
st.set_page_config(page_title="Sales Dashboard", layout="wide")

# Custom CSS for Maroon Theme
st.markdown("""
    <style>
    .main { background-color: #a82444; color: white; }
    h1, h2, h3 { color: white !important; }
    .stSelectbox, .stMultiSelect { color: black; }
    </style>
    """, unsafe_allow_html=True)

# --- Dummy Data Generation (Aap apni CSV file yahan load kar sakte hain) ---
data = {
    'Category': ['Phones', 'Chairs', 'Storage', 'Tables', 'Binders', 'Machines', 'Accessories'],
    'Sales': [330007.1, 328167.76, 223843.59, 206965.68, 203412.77, 189238.68, 167380.31],
    'Year': [2014, 2015, 2016, 2017, 2014, 2015, 2016],
    'Profit': [21492.95, 33503.97, 39774.1, 50684.64, 25000, 31000, 42000],
    'Month': ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'],
    'MonthlySales': [3143.29, 1608.51, 32511.17, 9195.44, 9599.86, 8435.95, 8004.14, 9209.67, 30537.57, 11938.02, 30201.42, 20893.22]
}

df = pd.DataFrame({'Category': data['Category'], 'Sales': data['Sales']})
df_profit = pd.DataFrame({'Year': [2014, 2015, 2016, 2017], 'Profit': [21492.95, 33503.97, 39774.1, 50684.64]})
df_monthly = pd.DataFrame({'Month': data['Month'], 'Sales': data['MonthlySales']})

# --- Header ---
st.title("📊 Sale Dashboard")
c1, c2 = st.columns(2)
with c1: st.multiselect("Category", ['Furniture', 'Office Supplies', 'Technology'], default='Technology')
with c2: st.multiselect("Year", [2014, 2015, 2016, 2017], default=2014)

st.markdown("---")

# --- First Row ---
col1, col2, col3 = st.columns([1, 1.5, 1])

with col1:
    st.subheader("Sale Category")
    fig1 = px.bar(df, x='Sales', y='Category', orientation='h', template="plotly_dark", color_discrete_sequence=['#d63384'])
    st.plotly_chart(fig1, use_container_width=True)

with col2:
    st.subheader("Profit Gained Over Time")
    fig2 = px.line(df_profit, x='Year', y='Profit', markers=True, template="plotly_dark")
    st.plotly_chart(fig2, use_container_width=True)

with col3:
    st.subheader("Customer Count")
    fig3 = px.bar(df_profit, x='Profit', y='Year', orientation='h', template="plotly_dark")
    st.plotly_chart(fig3, use_container_width=True)

# --- Second Row ---
col4, col5 = st.columns([1, 2])

with col4:
    st.subheader("Top 5 Customers")
    fig4 = px.pie(names=['Tamara', 'Raymond', 'Sanjit', 'Hunter', 'Adrian'], values=[24, 19, 15, 15, 14], hole=0.4, template="plotly_dark")
    st.plotly_chart(fig4, use_container_width=True)

with col5:
    st.subheader("Monthly Sales")
    fig5 = px.area(df_monthly, x='Month', y='Sales', template="plotly_dark", color_discrete_sequence=['white'])
    st.plotly_chart(fig5, use_container_width=True)
    
