import pandas as pd
from sklearn.cluster import KMeans
from jinja2 import Template


# -------------------------------
# Sample customer dataset
# -------------------------------
data = {
    'customer_id': [1, 2, 3, 4],
    'name': ['Alice', 'Bob', 'Charlie', 'Diana'],
    'email': ['alice@example.com', 'bob@example.com', 'charlie@example.com', 'diana@example.com'],
    'age': [25, 45, 35, 28],
    'purchase_history': [300, 1500, 800, 200]  # Total spent
}
df = pd.DataFrame(data)

# -------------------------------
# Segment customers with KMeans
# -------------------------------
def segment_customers(df, n_clusters=2):
    model = KMeans(n_clusters=n_clusters, random_state=42)
    df['segment'] = model.fit_predict(df[['purchase_history']])
    return df

# -------------------------------
# Personalized recommendation logic
# -------------------------------
def get_recommendation(segment_id):
    recommendations = {
        0: "Enjoy 10% off on new arrivals!",
        1: "Exclusive offers on premium collections just for you!"
    }
    return recommendations.get(segment_id, "Explore our newest products!")

# -------------------------------
# Email template generator
# -------------------------------
def generate_email(name, recommendation):
    template_str = """
    Hi {{ name }},

    {{ recommendation }}

    Visit us today at www.example.com!

    Cheers,  
    Your Favorite Brand
    """
    template = Template(template_str.strip())
    return template.render(name=name, recommendation=recommendation)

# -------------------------------
# Mock email sending function
# -------------------------------
def send_email(to_address, subject, body):
    print("="*60)
    print(f"TO: {to_address}")
    print(f"SUBJECT: {subject}")
    print(body)
    print("="*60)

# -------------------------------
# Main function
# -------------------------------
def main():
    segmented_df = segment_customers(df)

    for _, row in segmented_df.iterrows():
        recommendation = get_recommendation(row['segment'])
        email_body = generate_email(row['name'], recommendation)
        send_email(row['email'], "Your Personalized Offer", email_body)

# -------------------------------
# Run the script
# -------------------------------
if _name_ == "_main_":
    main()
