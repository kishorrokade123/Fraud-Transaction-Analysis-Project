# Fraud-Transaction-Analysis-Project
I have created an app interface where you can enter the transaction details and can get the prediction whether the said transaction is "Fraud" or not.
I have attached Jupyter notebook analysis in Screenshot and below is the app frontend code using streamlit -

import streamlit as st
import pandas as pd
import joblib

model = joblib.load("fraud_detection_pipeline.pkl")

st.title("Fraud Detection Predection App")

st.markdown("Please enter transaction details and enter predict button")

st.divider()

transaction_type = st.selectbox("Transaction Type", ["PAYMENT","TRANSFER","CASH_OUT","DEPOSIT"])
amount = st.number_input("Amount",min_value = 0.0,value = 1000.0)
oldbalanceOrg = st.number_input("Old Balance(Sender)",min_value = 0.0,value=10000.0)
newbalanceOrg = st.number_input("New Balance(Sender)",min_value = 0.0,value=9000.0)
oldbalanceDest = st.number_input("Old Balance(Receiver)",min_value = 0.0,value=0.0)
newbalanceDest = st.number_input("New Balance(Receiver)",min_value = 0.0,value=0.0)

if st.button("Predict"):
    input_data = pd.DataFrame([{
        "type": transaction_type,
        "amount": amount,
        "oldbalanceOrg": oldbalanceOrg,
        "newbalanceOrig": newbalanceOrg,
        "oldbalanceDest": oldbalanceDest,
        "newbalanceDest": newbalanceDest
    }])

    prediction = model.predict(input_data)[0]
    st.subheader(f"Prediction:'{int(prediction)}'")

    if prediction == 1:
        st.error("This transaction can be fraud")
    else:
        st.error("This transaction looks like it is not a fraud")






