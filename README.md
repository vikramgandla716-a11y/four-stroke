# four-stroke
import math
import streamlit as st


# ------------------------------------------------
# Page Configuration
# ------------------------------------------------
st.set_page_config(
    page_title="4-Stroke Engine Calculator",
    page_icon="⚙️",
    layout="centered"
)


# ------------------------------------------------
# Title
# ------------------------------------------------
st.title("⚙️ 4-Stroke Engine Calculator")
st.write("Calculate engine displacement, power, BHP, BMEP and piston speed.")


# ------------------------------------------------
# Input Section
# ------------------------------------------------
st.header("🔧 Engine Inputs")

col1, col2 = st.columns(2)

with col1:
    bore_mm = st.number_input(
        "Bore Diameter (mm)",
        min_value=1.0,
        value=80.0,
        step=0.1
    )

    stroke_mm = st.number_input(
        "Stroke Length (mm)",
        min_value=1.0,
        value=90.0,
        step=0.1
    )

    cylinders = st.number_input(
        "Number of Cylinders",
        min_value=1,
        value=4,
        step=1
    )


with col2:
    rpm = st.number_input(
        "Engine RPM",
        min_value=1.0,
        value=3000.0,
        step=100.0
    )

    torque = st.number_input(
        "Torque (Nm)",
        min_value=0.0,
        value=150.0,
        step=1.0
    )

    compression_ratio = st.number_input(
        "Compression Ratio",
        min_value=1.1,
        value=10.0,
        step=0.1
    )


# ------------------------------------------------
# Calculate Button
# ------------------------------------------------
if st.button("🚀 Calculate", type="primary"):

    # --------------------------------------------
    # 1. Swept Volume per Cylinder
    # --------------------------------------------
    swept_volume_mm3 = (
        (math.pi / 4)
        * bore_mm ** 2
        * stroke_mm
    )

    swept_volume_cc = swept_volume_mm3 / 1000

    # --------------------------------------------
    # Total Engine Displacement
    # --------------------------------------------
    total_displacement_cc = (
        swept_volume_cc * cylinders
    )

    # --------------------------------------------
    # 2. Clearance Volume
    # --------------------------------------------
    clearance_volume_cc = (
        swept_volume_cc
        / (compression_ratio - 1)
    )

    # --------------------------------------------
    # 3. Total Cylinder Volume
    # --------------------------------------------
    total_cylinder_volume_cc = (
        swept_volume_cc
        + clearance_volume_cc
    )

    # --------------------------------------------
    # 4. Mean Piston Speed
    # --------------------------------------------
    stroke_m = stroke_mm / 1000

    mean_piston_speed = (
        2 * stroke_m * rpm
    ) / 60

    # --------------------------------------------
    # 5. Engine Power
    # --------------------------------------------
    power_watts = (
        2 * math.pi * rpm * torque
    ) / 60

    power_kw = power_watts / 1000

    # --------------------------------------------
    # 6. Brake Horse Power
    # --------------------------------------------
    bhp = power_watts / 745.7

    # --------------------------------------------
    # 7. BMEP
    # --------------------------------------------
    displacement_m3 = (
        total_displacement_cc * 1e-6
    )

    bmep_pa = (
        4 * math.pi * torque
    ) / displacement_m3

    bmep_mpa = bmep_pa / 1e6

    # ------------------------------------------------
    # Results
    # ------------------------------------------------
    st.success("Calculation completed successfully!")

    st.header("📊 Engine Results")

    # First row
    col1, col2, col3 = st.columns(3)

    with col1:
        st.metric(
            "Displacement / Cylinder",
            f"{swept_volume_cc:.2f} cc"
        )

    with col2:
        st.metric(
            "Total Displacement",
            f"{total_displacement_cc:.2f} cc"
        )

    with col3:
        st.metric(
            "Clearance Volume",
            f"{clearance_volume_cc:.2f} cc"
        )

    # Second row
    col1, col2, col3 = st.columns(3)

    with col1:
        st.metric(
            "Piston Speed",
            f"{mean_piston_speed:.2f} m/s"
        )

    with col2:
        st.metric(
            "Power",
            f"{power_kw:.2f} kW"
        )

    with col3:
        st.metric(
            "BHP",
            f"{bhp:.2f} HP"
        )

    # Third row
    col1, col2 = st.columns(2)

    with col1:
        st.metric(
            "BMEP",
            f"{bmep_mpa:.2f} MPa"
        )

    with col2:
        st.metric(
            "Total Cylinder Volume",
            f"{total_cylinder_volume_cc:.2f} cc"
        )

    # ------------------------------------------------
    # Formula Section
    # ------------------------------------------------
    st.divider()

    st.header("📐 Formulas Used")

    st.latex(
        r"V_s = \frac{\pi}{4}D^2L"
    )

    st.latex(
        r"CR = \frac{V_s + V_c}{V_c}"
    )

    st.latex(
        r"V_p = \frac{2LN}{60}"
    )

    st.latex(
        r"P = \frac{2\pi NT}{60}"
    )

    st.latex(
        r"BMEP = \frac{4\pi T}{V_d}"
    )
