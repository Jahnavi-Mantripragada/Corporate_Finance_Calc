import streamlit as st
import pandas as pd

def calculate_npv(cash_flows, discount_rate):
    initial_investment = cash_flows[0]
    npv = sum(
        cf / ((1 + discount_rate) ** t) for t, cf in enumerate(cash_flows[1:], start=1)
    ) + initial_investment
    return npv

def main():
    st.title("Net Present Value (NPV) Calculator")
    st.write("This app helps you calculate the NPV of multiple projects based on your cash flows and discount rate.")

    # Initialize session state for projects
    if 'projects' not in st.session_state:
        st.session_state.projects = {}

    st.subheader("Enter Cash Flows for Projects")

    # Input project name
    project_name = st.text_input("Enter Project Name:")

    # Input cash flows dynamically
    cash_flow_input = st.text_input("Enter cash flows separated by commas (e.g., -20, 10, 10, 20, 30):")

    # Add project button
    if st.button("Add Project"):
        if project_name and cash_flow_input:
            try:
                cash_flows = [float(x.strip()) for x in cash_flow_input.split(',')]
                st.session_state.projects[project_name] = cash_flows
                st.success(f"Project '{project_name}' added successfully!")
            except ValueError:
                st.error("Invalid cash flow input. Ensure all values are numbers separated by commas.")
        else:
            st.error("Please provide both a project name and cash flows.")

    # Display all projects
    if st.session_state.projects:
        st.subheader("Current Projects")
        project_data = {
            "Project Name": [],
            "Cash Flows": []
        }
        for name, flows in st.session_state.projects.items():
            project_data["Project Name"].append(name)
            project_data["Cash Flows"].append(flows)

        df = pd.DataFrame(project_data)
        st.table(df)

    # Input for discount rate
    discount_rate = st.number_input(
        "Enter Discount Rate (in %):", min_value=0.0, max_value=100.0, value=10.0, step=0.1
    )

    # Calculate NPV for all projects
    if st.button("Calculate NPVs"):
        if not st.session_state.projects:
            st.error("Please add at least one project before calculating NPVs.")
        else:
            discount_rate_decimal = discount_rate / 100
            npv_results = {
                "Project Name": [],
                "NPV": []
            }
            for name, flows in st.session_state.projects.items():
                npv = calculate_npv(flows, discount_rate_decimal)
                npv_results["Project Name"].append(name)
                npv_results["NPV"].append(npv)

            npv_df = pd.DataFrame(npv_results)
            st.subheader("NPV Results")
            st.table(npv_df)

if __name__ == "__main__":
    main()
