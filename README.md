# SXD-Technical-Challenge
## Question 1

Firstly, we know that x1 and x2 must be greater than or equal to 0. Secondly, since we are minimizing Z which involves the sum of a negative and positive term (we know the first term is negative since x1 must be >= 0 and 3 is > 0 so negative * positive * positive = negative), our objective is to ultimately include as many of the negative terms as possible, and as few of the positive term as possible. We will find the exact amounts by looking at our constraints.

In this case, since x2 >= 0 and we want x2 to be as small as possible, we already know that x2 must be 0 to minimze Z (anything positive only increases Z). To find the optimal x1 value, since x2 is 0, we are effectively solving for both: x1 <= 5 and 2x1 <= 8 and taking the minimum of these two answers. We take the minimum since both must hold true, not just one of the constraint conditions.

Solving both, we get answers of x1 = 5 and x1 = 4 (these are the largest possible x1 values given their respective constraints and that we wish to maximize x1). Taking the minimum, we find that x1 = 4.

Now that we have found both x1 and x2, we can confirm that these values satisfy all the constraints:
4 + 0 = 4 <= 5 is true
2(4) + 0 = 8 <= 8 is true
4 >= 0 is true
0 >= 0 is true

Min Z = -3(4) + 0 = -12

**Hence, the answer is: (4, 0, -12) for (x1, x2, Z)**


## Question 2
Commented notebook and Python source code is provided in the Github Repository.

**Answer: (8, 18, 96) for (x1, x2, Z)**


## Question 3
To store and retrieve these values into a database with sub-millisecond latency as described, I would choose to use a NoSQL database, specifically one with a key-value store (keys being (Max, 3, 4) and value being the Maximum of 3x1 + 4x2 = 96). To do this, we could use a structure/system such as Redis, since it has high-performance and low-latency data retrieval. Redis also supports atomic operations, making it easy to update and retrieve multiple values in a single transaction.

To accomodate for scaling to a large size (target audience of math students in the US), I would first attempt to estimate an upper bound on the number of such unique linear programming problems that are likely to be solved, and the number of users that would attempt to solve them. Given the nature of these problems, I would potentially expect a higher number of users to attempt to solve them than problems that exist. To account for this, I would choose a database instance with sufficient memory to store all the expected sets of coefficients. To help with scaling and handling the large expected number of users, I would consider sharding the database across multiple instances.

To prevent overloading a single database instance with requests, I would consider implementing caching strategies such as setting a TTL (time-to-live) for cached values, using a load balancer to distribute requests across multiple instances, and implementing rate limiting to prevent abusive usage. 

Finally, for deployment, I would choose a cloud infrastructure to host the database, such as Amazon Web Services (AWS) or Microsoft Azure, to take advantage of their managed database services and scalability features. By using a managed database service, I would not need to worry about setting up and maintaining the database infrastructure, and could instead focus on developing and deploying the application itself. In addition, it would be easier for the app to run on the webpage than a localized database instance.

Below is an example of an integration test we could perform for pattern matching:


```
import unittest
import requests

class TestBackend(unittest.TestCase):
    def test_existing_problem(self):
        # set up test data
        url = "http://backend.com/api/check-problem"
        payload = {
            "max_min": "max",
            "coefficients": [3, 4],
            "constraints": [
                {"coefficients": [1, 1], "operator": "<=", "rhs": 5},
                {"coefficients": [2, 1], "operator": "<=", "rhs": 8},
                {"coefficients": [1, 0], "operator": ">=", "rhs": 0},
            ]
        }
        # make request to check if problem exists
        response = requests.post(url, json=payload)
        # check if response is successful and problem exists in backend
        self.assertEqual(response.status_code, 200)
        self.assertTrue(response.json()["exists"])
```

In this test, we make a POST request to the /api/check-problem endpoint (assume its called check-problem) with a payload that represents a problem pattern. We are checking if this problem pattern already exists in the backend by expecting a response with an HTTP status code of 200 and a JSON object with a boolean field "exists" that is True if the problem exists in the backend and False otherwise (I chose to use JSON in accordance with the NoSQL database instance mentioned above).
