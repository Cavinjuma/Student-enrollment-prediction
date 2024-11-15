import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, classification_report
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
import pandas as pd
import numpy as np
# Set random seed
np.random.seed(42)
# Number of records to generate
num_records = 1000
# Enrollment Data
enrollment_data = pd.DataFrame({
    'student_id': np.arange(1, num_records + 1),
    'previous_enrollments': np.random.randint(0, 5, num_records),  # number of times previously enrolled
    'enrolled': np.random.choice([0, 1], num_records, p=[0.3, 0.7])  # 0 for not enrolled, 1 for enrolled
})
enrollment_data.to_csv("enrollment_data.csv", index=False)
# Academic Records
academic_data = pd.DataFrame({
    'student_id': np.arange(1, num_records + 1),
    'gpa': np.round(np.random.uniform(2.0, 4.0, num_records), 2),  # GPA between 2.0 and 4.0
    'attendance_rate': np.round(np.random.uniform(0.6, 1.0, num_records), 2)  # Attendance rate between 60% and 100%
})
academic_data.to_csv("academic_data.csv", index=False)
# Demographic Data
demographic_data = pd.DataFrame({
    'student_id': np.arange(1, num_records + 1),
    'age': np.random.randint(18, 30, num_records),  # Age between 18 and 30
    'gender': np.random.choice(['Male', 'Female', 'Non-Binary'], num_records),
    'ethnicity': np.random.choice(['Hispanic', 'Non-Hispanic White', 'Non-Hispanic Black', 'Asian', 'Other'], num_records),
    'income_level': np.random.choice(['Low', 'Medium', 'High'], num_records),
    'program': np.random.choice(['Science', 'Arts', 'Business', 'Engineering'], num_records)
})
demographic_data.to_csv("demographic_data.csv", index=False)
print("Sample CSV files created: enrollment_data.csv, academic_data.csv, demographic_data.csv")
# Load data
enrollment_data = pd.read_csv("enrollment_data.csv")
academic_data = pd.read_csv("academic_data.csv")
demographic_data = pd.read_csv("demographic_data.csv")
# Preview the data
print(enrollment_data.head())
print(academic_data.head())
print(demographic_data.head())
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
# Merge datasets on student ID
data = pd.merge(enrollment_data, academic_data, on="student_id")
data = pd.merge(data, demographic_data, on="student_id")
# Handle missing values for numerical features only
numeric_features = data.select_dtypes(include=np.number).columns
data[numeric_features] = data[numeric_features].fillna(data[numeric_features].mean())
# Encode categorical variables including income_level
data = pd.get_dummies(data, columns=['gender', 'ethnicity', 'program', 'income_level']) # Added income_level
# Feature selection
# Updated features to include the encoded income_level columns
features = ['previous_enrollments', 'gpa', 'attendance_rate', 'age', 
            'income_level_High', 'income_level_Low', 'income_level_Medium']  
target = 'enrolled'  # column indicating whether the student enrolled
X = data[features]
y = data[target]
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42) 
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, classification_report
model = RandomForestClassifier(random_state=42) # Initialize the model
model.fit(X_train, y_train) 
# Make predictions on the test set
y_pred = model.predict(X_test) 
# Calculate accuracy
accuracy = accuracy_score(y_test, y_pred)
print("Accuracy:", accuracy)
# Print classification report for more detailed metrics
print(classification_report(y_test, y_pred))
import matplotlib.pyplot as plt
# Feature importance visualization
importances = model.feature_importances_
indices = np.argsort(importances)[::-1]
plt.figure()
plt.title("Feature Importances")
plt.bar(range(X.shape[1]), importances[indices], color="b", align="center")
plt.xticks(range(X.shape[1]), [features[i] for i in indices], rotation=90)
plt.show()
