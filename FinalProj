import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("Used_cars.csv")
df = df.dropna()  # used this to make the data clean so, I got rid of the NA's


def home_page():
    st.title("Curran's Car Sales & More")
    st.caption("Powered by: Craigslist")
    st.subheader("About this page:")
    st.image("Cars.webp")
    st.write("This page is created to help you learn a little bit about the used car market and also provide you "
             "with some unique tools to find a car yourself.")
    st.write("Use the dropdown bar to the left to navigate this site.")


def overview_page():
    st.title("Data Overview")
    st.write("This section is here to give you a overview of all the data we have in the database.")
    st.write("I have removed all #NA values effectively removing all the bad listings. In my experience incomplete "
             "listings, or 'Ghost Posts' never seem to pan out and are worth ignoring.")
    st.write(df)


def demographics_page():
    st.title("Car Demographics")
    st.write("This page has a few pie charts showing the breakdown of the cars for sale, describing the overall car "
             "conditions, fuel types, and drive types.")
    fig, ax = plt.subplots()
    condition_counts = df['condition'].value_counts()
    plt.pie(condition_counts,
            colors=["orange", "green", "blue", "purple", "red", "black"], explode=[0, 0, 0, 0.1, 0.3, 0.5],
            autopct='%1.2f%%', textprops={'size': 'xx-small'}, radius=0.5)
    plt.legend(labels=["Excellent", "Good", "Like New", "Salvage", "New", "Fair"], loc="upper right")
    plt.title("Car Conditions")
    st.pyplot(fig)

    fig2, ax1 = plt.subplots()
    plt.rc('font', size=10)
    condition_counts = df['fuel'].value_counts()
    plt.pie(condition_counts,
            colors=["red", "blue", "green", "purple"], explode=[0, 0, 0.2, 0.5], autopct='%1.2f%%',
            textprops={'size': 'xx-small'}, radius=0.5)
    plt.legend(labels=["Gas", "Diesel", "Hybrid", "Electric"], loc="upper right")
    plt.title("Car Fuel Types")
    st.pyplot(fig2)

    fig3, ax2 = plt.subplots()
    plt.rc('font', size=10)
    condition_counts = df['drive'].value_counts()
    plt.pie(condition_counts, labels=["4wd", "fwd", "rwd"],
            colors=["orange", "green", "blue"], autopct='%1.2f%%', textprops={'size': 'xx-small'}, radius=0.5)
    plt.title("Car Drive Types")
    st.pyplot(fig3)


def distribution_page():
    st.title("Distribution of Car Prices")
    st.write("This chart is a histogram of all the car prices in the dataset. It is interesting to see the "
             "distribution of all the prices. The majority of cars are listed at around the \$10,000 mark. There are "
             "also a lot of cars listed for close to nothing. The distribution slides down to around the \$40,000 mark "
             "with only a few outliers beyond there.")
    fig4, ax = plt.subplots()
    plt.hist(df['price'], bins=100)
    plt.xlabel("Price ($USD)")
    plt.ylabel("Number of Cars at That Price Point")
    plt.title("Car Prices Frequencies")
    st.pyplot(fig4)


def sales_by_state_page():
    st.title("Car Sales By State")
    st.write("This is a dynamic chart that will show the car sales by State. The x-axis is the year of the vehicle, and"
             " the y-axis is the Price. It would probably be expected that there would be a rising trend, as the cars "
             "get newer the prices gets higher. There may be some exceptions of older cars holding higher value (which "
             "would be able to see in the charting).")
    fig5, ax = plt.subplots()
    state = st.selectbox("Select State:", df.state.unique())
    filtered_data = df[(df.state == state)]
    plt.scatter(filtered_data.year, filtered_data.price)
    plt.xlabel("Year")
    plt.ylabel("Price ($USD)")
    plt.title("Car Prices By State")
    st.pyplot(fig5)


def lookup_page():
    st.title("Vin Look-Up")
    st.write("A VIN is a vehicle identification number. The automotive industry gives every car a unique code when it "
             "is made. This page can be a useful tool to see if someone stole your car and is trying to sell it. "
             "Take the VIN from your Title and input it here.")
    vin = st.text_input("Enter a VIN:")
    if vin in df['VIN'].values:  # check if the VIN is present in the DataFrame
        car = df.loc[df['VIN'] == vin]
        st.write("Your car has been found! Contact the police and provide them the details listed below to help get "
                 "it back.")
        st.map(car)
        st.write(car)
    else:
        st.write("Sorry, the VIN you entered is not in our Database. Please ensure you typed it correctly.")
        st.write("If you believe you car has been stolen and cannot find it, contact the police to report it stolen.")

    st.caption("Hint: Try 1FTFW1EF4EKF38157")


def car_finder_page():
    st.title("Car Finder")
    st.write("This page will be useful to any potential used car buyers. Select what you are looking for and find out "
             "where it is located. You will get a filtered list of what you are looking for and now know where to go!")
    st.caption("Hint: Try Ram, 1500, 2015, gray-black-white, excellent-good, automatic, 4wd")
    make = st.selectbox("Select car make:", df.manufacturer.unique())
    model = st.selectbox("Select car model:", df.model.unique())
    year = st.multiselect("Select year range:", df.year.unique())
    paint_color = st.multiselect("Select Color: ", df.paint_color.unique())
    condition = st.multiselect("Select Condition: ", df.condition.unique())
    transmission = st.multiselect("Select Transmission: ", df.transmission.unique())
    drive = st.multiselect("Select Drive: ", df.drive.unique())
    filtered_data = df[(df.manufacturer == make) & (df.model == model) & (df.year.isin(year).any()) &
                       (df.paint_color.isin(paint_color).any()) & (df.condition.isin(condition).any()) &
                       (df.transmission.isin(transmission).any()) & (df.drive.isin(drive).any())]
    st.map(filtered_data)
    st.write(filtered_data)


def main():
    option = st.sidebar.selectbox("Navigation Bar", ("Home", "Data Overview", "Car Demographics",
                                                 "Distribution of Car Prices", "Car Sales By State",
                                                 "Stolen Car Look-Up", "Car Finder"))
    if option == "Home":
        home_page()
    elif option == "Data Overview":
        overview_page()
    elif option == "Car Demographics":
        demographics_page()
    elif option == "Distribution of Car Prices":
        distribution_page()
    elif option == "Car Sales By State":
        sales_by_state_page()
    elif option == "Stolen Car Look-Up":
        lookup_page()
    elif option == "Car Finder":
        car_finder_page()


main()
