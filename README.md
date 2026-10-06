import streamlit as st
import math

st.title("⚙️ 4-Stroke Engine Calculator")

bore = st.number_input("Bore (mm)", value=80.0)
stroke = st.number_input("Stroke (mm)", value=90.0)
cyl = st.number_input("Cylinders", value=4, step=1)
rpm = st.number_input("RPM", value=3000.0)
torque = st.number_input("Torque (Nm)", value=150.0)
cr = st.number_input("Compression Ratio", value=10.0)

if st.button("Calculate"):

    # Displacement
    swept = (math.pi / 4) * bore**2 * stroke / 1000
    displacement = swept * cyl

    # Clearance volume
    clearance = swept / (cr - 1)

    # Piston speed
    piston_speed = 2 * (stroke / 1000) * rpm / 60

    # Power
    power = (2 * math.pi * rpm * torque) / 60000

    # BHP
    bhp = power / 0.7457

    # BMEP
    bmep = (4 * math.pi * torque) / (displacement * 1e-6) / 1e6

    st.subheader("Results")

    st.write("Swept Volume:", round(swept, 2), "cc")
    st.write("Displacement:", round(displacement, 2), "cc")
    st.write("Clearance Volume:", round(clearance, 2), "cc")
    st.write("Piston Speed:", round(piston_speed, 2), "m/s")
    st.write("Power:", round(power, 2), "kW")
    st.write("BHP:", round(bhp, 2), "HP")
    st.write("BMEP:", round(bmep, 2), "MPa")
