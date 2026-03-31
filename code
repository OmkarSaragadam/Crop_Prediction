import streamlit as st

# ---------- Page Config ----------
st.set_page_config(
    page_title="CropSense – AI-Powered Crop Intelligence",
    page_icon="🌱",
    layout="centered"
)

# ---------- Custom CSS ----------
st.markdown("""
<style>
    @import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;600;700&display=swap');

    html, body, [class*="css"] {
        font-family: 'DM Sans', sans-serif;
    }

    .main {
        background: linear-gradient(135deg, #e8f5e9 0%, #f1f8e9 100%);
        min-height: 100vh;
    }

    .block-container {
        padding-top: 2rem;
        padding-bottom: 2rem;
    }

    .logo-area {
        text-align: center;
        margin-bottom: 2rem;
    }

    .logo-area h1 {
        font-size: 2.4rem;
        font-weight: 700;
        color: #1b5e20;
        margin: 0;
    }

    .logo-area p {
        color: #4caf50;
        font-size: 0.85rem;
        letter-spacing: 2px;
        font-weight: 600;
        margin-top: 0.2rem;
    }

    .card {
        background: white;
        border-radius: 20px;
        padding: 2rem 2.5rem;
        box-shadow: 0 4px 24px rgba(0,0,0,0.07);
    }

    .card h2 {
        font-size: 1.4rem;
        font-weight: 700;
        color: #1b5e20;
        border-left: 4px solid #2e7d32;
        padding-left: 0.75rem;
        margin-bottom: 1.5rem;
    }

    .result-box {
        background: linear-gradient(135deg, #2e7d32, #1b5e20);
        border-radius: 16px;
        padding: 1.5rem 2rem;
        color: white;
        text-align: center;
        margin-top: 1.5rem;
    }

    .result-box h3 {
        font-size: 1rem;
        letter-spacing: 2px;
        opacity: 0.8;
        margin-bottom: 0.5rem;
    }

    .result-box .crop-name {
        font-size: 2rem;
        font-weight: 700;
        margin: 0;
    }

    .result-box .crop-detail {
        font-size: 0.9rem;
        opacity: 0.85;
        margin-top: 0.5rem;
    }

    .stButton > button {
        background: linear-gradient(135deg, #2e7d32, #1b5e20);
        color: white;
        font-size: 1rem;
        font-weight: 700;
        letter-spacing: 1.5px;
        border: none;
        border-radius: 12px;
        padding: 0.85rem 2rem;
        width: 100%;
        margin-top: 1.5rem;
        cursor: pointer;
        transition: opacity 0.2s;
    }

    .stButton > button:hover {
        opacity: 0.9;
    }

    label {
        font-size: 0.75rem !important;
        font-weight: 700 !important;
        letter-spacing: 1.5px !important;
        color: #2e7d32 !important;
        text-transform: uppercase !important;
    }
</style>
""", unsafe_allow_html=True)


# ---------- Rule-Based Prediction ----------
def predict_crop(ec, ph, temp, moisture):
    """
    Rule-based crop prediction from soil and field parameters.
    Returns (crop_name, reason, emoji)
    """
    crops = []

    # Rice: high moisture, moderate-high temp, slightly acidic
    if moisture >= 60 and 5.5 <= ph <= 7.0 and temp >= 22:
        crops.append(("Rice 🌾", "Thrives in high moisture, warm temps, and near-neutral pH.", ec <= 2.0))

    # Wheat: moderate moisture, cool-moderate temp, neutral pH
    if 40 <= moisture <= 65 and 6.0 <= ph <= 7.5 and 15 <= temp <= 25:
        crops.append(("Wheat 🌾", "Prefers moderate moisture, cooler temperatures, and neutral soil.", ec <= 1.5))

    # Sugarcane: high moisture, warm, moderate EC
    if moisture >= 55 and temp >= 25 and 6.0 <= ph <= 7.5 and 0.5 <= ec <= 2.5:
        crops.append(("Sugarcane 🎋", "Needs warm climate, high moisture, and moderate soil salinity.", True))

    # Cotton: low-moderate moisture, warm, slightly alkaline
    if 30 <= moisture <= 55 and temp >= 25 and 6.5 <= ph <= 8.0 and ec <= 3.0:
        crops.append(("Cotton 🌸", "Suits warm, semi-arid conditions with well-drained soil.", True))

    # Maize: moderate moisture, warm, near-neutral pH
    if 40 <= moisture <= 65 and 20 <= temp <= 32 and 5.8 <= ph <= 7.2:
        crops.append(("Maize 🌽", "Ideal for warm, well-drained soils with moderate moisture.", ec <= 2.5))

    # Groundnut: low-moderate moisture, warm, slightly acidic
    if 30 <= moisture <= 55 and temp >= 22 and 5.5 <= ph <= 7.0:
        crops.append(("Groundnut 🥜", "Grows well in sandy loam, warm temps, and slightly acidic soils.", ec <= 2.0))

    # Tomato: moderate moisture, warm, slightly acidic
    if 40 <= moisture <= 60 and 20 <= temp <= 30 and 5.5 <= ph <= 7.0 and ec <= 2.5:
        crops.append(("Tomato 🍅", "Prefers moderate moisture, warm days, and slightly acidic soil.", True))

    # Potato: moderate moisture, cool, slightly acidic
    if 40 <= moisture <= 65 and 15 <= temp <= 22 and 5.0 <= ph <= 6.5:
        crops.append(("Potato 🥔", "Thrives in cool temps, moderate moisture, and acidic soil.", ec <= 2.0))

    # Soybean: moderate moisture, warm, neutral
    if 40 <= moisture <= 65 and 20 <= temp <= 30 and 6.0 <= ph <= 7.5:
        crops.append(("Soybean 🫘", "Suits warm, well-drained soils with moderate moisture.", ec <= 2.0))

    # Barley: tolerates high EC / salinity
    if ec >= 2.0 and 40 <= moisture <= 65 and 10 <= temp <= 24 and 6.0 <= ph <= 8.0:
        crops.append(("Barley 🌾", "One of the most salt-tolerant grains; suits cooler climates.", True))

    # Filter by EC suitability flag
    suitable = [(name, reason) for name, reason, ec_ok in crops if ec_ok]

    if suitable:
        return suitable[0]
    elif crops:
        return crops[0][0], crops[0][1]
    else:
        return "No Match Found ❌", "The entered parameters don't match common crop profiles. Try adjusting pH, moisture, or temperature."


# ---------- UI ----------
st.markdown("""
<div class="logo-area">
    <h1>🌱 CropSense</h1>
    <p>AI-POWERED CROP INTELLIGENCE</p>
</div>
""", unsafe_allow_html=True)

st.markdown('<div class="card">', unsafe_allow_html=True)
st.markdown('<h2>Enter Soil and Field Parameters</h2>', unsafe_allow_html=True)

col1, col2 = st.columns(2)

with col1:
    ec = st.number_input(
        "ELECTRICAL CONDUCTIVITY (dS/m)",
        min_value=0.0, max_value=20.0,
        value=1.2, step=0.1,
        help="Soil electrical conductivity in dS/m"
    )
    temp = st.number_input(
        "SOIL TEMPERATURE (°C)",
        min_value=0.0, max_value=60.0,
        value=28.0, step=0.5,
        help="Soil temperature in Celsius"
    )

with col2:
    ph = st.number_input(
        "PH LEVEL (0–14)",
        min_value=0.0, max_value=14.0,
        value=6.5, step=0.1,
        help="Soil pH level"
    )
    moisture = st.slider(
        "SOIL MOISTURE (%)",
        min_value=0, max_value=100,
        value=50, step=1
    )

predict = st.button("🌾  PREDICT BEST CROP")

if predict:
    crop, reason = predict_crop(ec, ph, temp, moisture)
    st.markdown(f"""
    <div class="result-box">
        <h3>RECOMMENDED CROP</h3>
        <p class="crop-name">{crop}</p>
        <p class="crop-detail">{reason}</p>
    </div>
    """, unsafe_allow_html=True)

    with st.expander("📊 Input Summary"):
        st.write(f"**Electrical Conductivity:** {ec} dS/m")
        st.write(f"**pH Level:** {ph}")
        st.write(f"**Soil Temperature:** {temp} °C")
        st.write(f"**Soil Moisture:** {moisture}%")

st.markdown('</div>', unsafe_allow_html=True)

st.markdown("""
---
<center style='color:#aaa; font-size:0.8rem;'>CropSense · Rule-based Crop Intelligence · Built with Streamlit</center>
""", unsafe_allow_html=True)
