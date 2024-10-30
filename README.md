import json
def decode_base(value_str, base):
    """Decode a value given in a specific base to a decimal integer."""
    return int(value_str, base)
def lagrange_interpolation(points):
    """Perform Lagrange interpolation to find the constant term (c) of the polynomial."""
    constant_term = 0.0
    for i, (xi, yi) in enumerate(points):
        term = yi  
        for j, (xj, _) in enumerate(points):
            if i != j:
                term *= (0 - xj) / (xi - xj)  
        constant_term += term

    return int(constant_term)
def find_secret_constant(json_input):
    """Decode values from JSON input and compute the constant term using interpolation."""
    data = json.loads(json_input)  
    n = data["keys"]["n"]
    k = data["keys"]["k"]
    points = []
    for key, value_data in data.items():
        if key != "keys":
            x = int(key)  
            base = int(value_data["base"])
            y = decode_base(value_data["value"], base) 
            points.append((x, y))
    if len(points) < k:
        raise ValueError("Insufficient number of points to solve for the polynomial.")
    secret = lagrange_interpolation(points[:k])
    return secret
json_input_1 = '''{
    "keys": {
        "n": 4,
        "k": 3
    },
    "1": {
        "base": "10",
        "value": "4"
    },
    "2": {
        "base": "2",
        "value": "111"
    },
    "3": {
        "base": "10",
        "value": "12"
    },
    "6": {
        "base": "4",
        "value": "213"
    }
}'''

secret_constant_1 = find_secret_constant(json_input_1)
print(f"The secret constant term 'c' for the first test case is: {secret_constant_1}")
json_input_2 = '''{
    "keys": {
        "n": 10,
        "k": 7
    },
    "1": {
        "base": "6",
        "value": "13444211440455345511"
    },
    "2": {
        "base": "15",
        "value": "aed7015a346d63"
    },
    "3": {
        "base": "15",
        "value": "6aeeb69631c227c"
    },
    "4": {
        "base": "16",
        "value": "e1b5e05623d881f"
    },
    "5": {
        "base": "8",
        "value": "316034514573652620673"
    },
    "6": {
        "base": "3",
        "value": "2122212201122002221120200210011020220200"
    },
    "7": {
        "base": "3",
        "value": "20120221122211000100210021102001201112121"
    },
    "8": {
        "base": "6",
        "value": "20220554335330240002224253"
    },
    "9": {
        "base": "12",
        "value": "45153788322a1255483"
    },
    "10": {
        "base": "7",
        "value": "1101613130313526312514143"
    }
}'''

secret_constant_2 = find_secret_constant(json_input_2)
print(f"The secret constant term 'c' for the second test case is: {secret_constant_2}")
