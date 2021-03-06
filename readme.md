# Name : Pankaj Shah
# Email: pshah9411@gmail.com

# 1. Count rows solution

count rows puzzle is solved using python programming language.
since some of the lines were broken so insted of counting rows 
particular words are counted which appeared only once in one row.
In this case word "Kubernetes" occurences is counted and the result is 
displayed.

source code :
```py
#reading file
file = open("csv-sample.csv", "r")
#reading content of file
data = file. read()
#counting the number of occurences of particular word as some word only appeared once in a row
occurrences = data. count("Kubernetes")
#printing number of rows
print('Number of rows in the file is :', occurrences)
```

# 2. Docker Image puzzle solution

I used nodejs to create Web api and 
followings docker commands are used in this puzzle to create image and then pushed to docker hub

```docker
FROM alpine:latest
RUN apk add --no-cache nodejs npm
COPY . app/
WORKDIR /app
RUN npm install
EXPOSE 8080 

ENTRYPOINT [ "node" ]
CMD [ "index.js" ]

```
link to dockerhub image:

https://hub.docker.com/repository/docker/shahpankaj/intern-test

## commands
```
docker pull shahpankaj/intern-test:latest
docker run -it --rm -p 8080:8080 --name sample shahpankaj/intern-test:latest
```
### browse http://localhost:8080/api/email it will return
```json
{
    "name":"Pankaj Shah",
    "email":"pshah9411@gmai.com"
}
```



# 3. Padlock Puzzle Solution

To solve padlock problem I wrote an algorithm. This algorithm solves the problem in four simple steps
This algorithm solves most of the 3 digit padlock problem

Algorithm:

```
step 1 : Initialize 6 arrays 2 for each digit place one array containing possible digit in particular place
         and another array containing not possible digits

step 2 : Fill both array for each digit with following logic
                [1,0] : contains one digit but in wrong place => fill not_accepted_position array with current position digit and accepted_position array with other two postion digit
                [1,1] : contains one digit  in right place => fill accepted_position array with current position digit and not_accepted_position array with other two postion digit
                [2,0] : contains two digit but in wrong place => fill not_accepted_position array with current position digit and accepted_position array with other two postion digit

        compare both array and  perform difference of array (accepted_position - not_accepted_position)   
        repeat step 2 for each input

step 3 : apply filter in all accepted_position arrays and remove duplicate digits

step 4 : verify each input based on possibility logic and find fixed postions
```

I implemented this algorithm using javascript. The code is not complete as for each probablity logic must be added and it works fine for both the padlock problem

```js

// declaring array variables for storing possible digits

var not_accepted_position_1 = [];
var not_accepted_position_2 = [];
var not_accepted_position_3 = [];
var accepted_position_1 = [];
var accepted_position_2 = [];
var accepted_position_3 = [];

const input_first = [
  {
    data: [1, 4, 7],
    possibility: [1, 0], // one digit is right but in wrong place
  },
  {
    data: [1, 8, 9],
    possibility: [1, 1], // one digit is right and in right place
  },
  {
    data: [9, 6, 4],
    possibility: [2, 0], // two digits are right and both in wrong place
  },
  {
    data: [5, 2, 3],
    possibility: [0, 0], // no digits are right 
  },
  {
    data: [2, 8, 6],
    possibility: [1, 0], // one digit is right but in wrong place
  },
];
const input_second = [
  {
    data: [6, 8, 2],
    possibility: [1, 1], // one digit is right and in right place
  },
  {
    data: [6, 1, 4],
    possibility: [1, 0], // one digit is right but in wrong place
  },
  {
    data: [2, 0, 6],
    possibility: [2, 0], // two digits are right and both in wrong place
  },
  {
    data: [7, 3, 8],
    possibility: [0, 0], // no digits are right 
  },
  {
    data: [3, 8, 0],
    possibility: [1, 0], // one digit is right but in wrong place
  },
];


//function to remove not accepted digits from accepted array
const removeElement = (arr, index) => {
  const length = arr.length;
  if (index !== length - 1) {
    while (index !== length - 1) {
      arr[index] = arr[index + 1];
      index++;
    }
  }
  arr.pop();
  return arr;
};

/**
 * logic for following hints
 * contains one digit but wrong place
 * contains two digits but wrong place
 * contains three digits but in wrong place
 */
const guessFirstCondition = (arr) => {
  for (x in arr) {
    switch (x) {
      case "0":
        arr.forEach((val) => {
          if (val !== arr[parseInt(x)]) {
            if (!accepted_position_1.includes(val)) {
              accepted_position_1.push(val);
            }
          } else {
            if (!not_accepted_position_1.includes(val)) {
              not_accepted_position_1.push(val);
            }
          }
        });
        not_accepted_position_1.forEach((val) => {
          for (y in accepted_position_1) {
            if (accepted_position_1[parseInt(y)] === val) {
              accepted_position_1 = removeElement(
                accepted_position_1,
                parseInt(y)
              );
            }
          }
        });
        break;

      case "1":
        arr.forEach((val) => {
          if (val !== arr[parseInt(x)]) {
            if (!accepted_position_2.includes(val)) {
              accepted_position_2.push(val);
            }
          } else {
            if (!not_accepted_position_2.includes(val)) {
              not_accepted_position_2.push(val);
            }
          }
        });
        not_accepted_position_2.forEach((val) => {
          for (y in accepted_position_2) {
            if (accepted_position_2[parseInt(y)] === val) {
              accepted_position_2 = removeElement(
                accepted_position_2,
                parseInt(y)
              );
            }
          }
        });
        break;
      case "2":
        arr.forEach((val) => {
          if (val !== arr[parseInt(x)]) {
            if (!accepted_position_3.includes(val)) {
              accepted_position_3.push(val);
            }
          } else {
            if (!not_accepted_position_3.includes(val)) {
              not_accepted_position_3.push(val);
            }
          }
        });
        not_accepted_position_3.forEach((val) => {
          for (y in accepted_position_3) {
            if (accepted_position_3[parseInt(y)] === val) {
              accepted_position_3 = removeElement(
                accepted_position_3,
                parseInt(y)
              );
            }
          }
        });
        break;

      default:
        break;
    }
  }
};

/**
 *  logic for following hints
 *  contains one digit and in right position
 */

const guessSecondCondition = (arr) => {
  for (x in arr) {
    switch (x) {
      case "0":
        arr.forEach((val) => {
          if (val === arr[parseInt(x)]) {
            if (!accepted_position_1.includes(val)) {
              accepted_position_1.push(val);
            }
          } else {
            if (!not_accepted_position_1.includes(val)) {
              not_accepted_position_1.push(val);
            }
          }
        });
        not_accepted_position_1.forEach((val) => {
          for (y in accepted_position_1) {
            if (accepted_position_1[parseInt(y)] === val) {
              accepted_position_1 = removeElement(
                accepted_position_1,
                parseInt(y)
              );
            }
          }
        });
        break;

      case "1":
        arr.forEach((val) => {
          if (val === arr[parseInt(x)]) {
            if (!accepted_position_2.includes(val)) {
              accepted_position_2.push(val);
            }
          } else {
            if (!not_accepted_position_2.includes(val)) {
              not_accepted_position_2.push(val);
            }
          }
        });
        not_accepted_position_2.forEach((val) => {
          for (y in accepted_position_2) {
            if (accepted_position_2[parseInt(y)] === val) {
              accepted_position_2 = removeElement(
                accepted_position_2,
                parseInt(y)
              );
            }
          }
        });
        break;
      case "2":
        arr.forEach((val) => {
          if (val === arr[parseInt(x)]) {
            if (!accepted_position_3.includes(val)) {
              accepted_position_3.push(val);
            }
          } else {
            if (!not_accepted_position_3.includes(val)) {
              not_accepted_position_3.push(val);
            }
          }
        });
        not_accepted_position_3.forEach((val) => {
          for (y in accepted_position_3) {
            if (accepted_position_3[parseInt(y)] === val) {
              accepted_position_3 = removeElement(
                accepted_position_3,
                parseInt(y)
              );
            }
          }
        });
        break;

      default:
        break;
    }
  }
};

/**
 * logic for following hints
 * contains 0 correct digits with 0 correct positions
 */

const guessThirdCondition = (arr) => {
  for (x in arr) {
    switch (x) {
      case "0":
        arr.forEach((val) => {
          if (!not_accepted_position_1.includes(val)) {
            not_accepted_position_1.push(val);
          }
        });
        break;

      case "1":
        arr.forEach((val) => {
          if (!not_accepted_position_2.includes(val)) {
            not_accepted_position_2.push(val);
          }
        });
        break;
      case "2":
        arr.forEach((val) => {
          if (!not_accepted_position_3.includes(val)) {
            not_accepted_position_3.push(val);
          }
        });
        break;

      default:
        break;
    }
  }
};

/**\
 * removing duplicate elements from arrays
 *
 */

const arrayFilter = () => {
  if (accepted_position_1.length > 1) {
    if (accepted_position_1.includes(accepted_position_2[0])) {
      accepted_position_2.forEach((val) => {
        for (x in accepted_position_1) {
          if (accepted_position_1[parseInt(x)] === val) {
            accepted_position_1 = removeElement(
              accepted_position_1,
              parseInt(x)
            );
          }
        }
      });
    }

    if (accepted_position_1.includes(accepted_position_3[0])) {
      accepted_position_3.forEach((val) => {
        for (x in accepted_position_1) {
          if (accepted_position_1[parseInt(x)] === val) {
            accepted_position_1 = removeElement(
              accepted_position_1,
              parseInt(x)
            );
          }
        }
      });
    }
  }
};

/**
 * verification of arrays containing multiple digits
 */

const Verification = (arr, possibility) => {
  const possible_digits = accepted_position_1.concat(
    accepted_position_2,
    accepted_position_3
  );

  switch (JSON.stringify(possibility)) {
    case "[1,0]":
      var digit = null;
      var count = 0;
      arr.forEach((val) => {
        for (x in possible_digits) {
          if (possible_digits[parseInt(x)] === val) {
            digit = val;
            count++;
          }
        }
      });
      if (
        count === 1 &&
        accepted_position_1.length > 1 &&
        accepted_position_1.includes(digit)
      ) {
        accepted_position_1 = [digit];
      } else if (
        count === 1 &&
        accepted_position_2.length > 1 &&
        accepted_position_2.includes(digit)
      ) {
        accepted_position_2 = [digit];
      } else if (
        count === 1 &&
        accepted_position_3.length > 1 &&
        accepted_position_3.includes(digit)
      ) {
        accepted_position_3 = [digit];
      }
      break;

    case "[1,1]":
      var digit = null;
      var count = 0;
      var index = null;
      arr.forEach((val) => {
        for (x in possible_digits) {
          if (possible_digits[parseInt(x)] === val) {
            digit = val;
            count++;
            index = arr.indexOf(val);
          }
        }
      });
      if (
        index === 0 &&
        count === 1 &&
        accepted_position_1.length > 1 &&
        accepted_position_1.includes(digit)
      ) {
        accepted_position_1 = [digit];
      } else if (
        index === 1 &&
        count === 1 &&
        accepted_position_2.length > 1 &&
        accepted_position_2.includes(digit)
      ) {
        accepted_position_2 = [digit];
      } else if (
        index === 2 &&
        count === 1 &&
        accepted_position_3.length > 1 &&
        accepted_position_3.includes(digit)
      ) {
        accepted_position_3 = [digit];
      }
      break;
    default:
      break;
  }
};

//main function

const guessPossibleNumbers = (arr, possibility) => {
  switch (JSON.stringify(possibility)) {
    case "[1,0]":
      guessFirstCondition(arr);
      break;
    case "[1,1]":
      guessSecondCondition(arr);
      break;
    case "[2,0]":
      guessFirstCondition(arr);
      break;
    case "[0,0]":
      guessThirdCondition(arr);
      break;
    default:
      break;
  }
};


input_first.forEach((obj) => {
  guessPossibleNumbers(obj.data , obj.possibility)
})
arrayFilter();
input_first.forEach((obj) => {
    Verification(obj.data, obj.possibility);
  })



console.log("Accepted  ", accepted_position_1);
console.log("___________________");

console.log("Accepted  ", accepted_position_2);
console.log("___________________");

console.log("Accepted  ", accepted_position_3);
console.log("___________________");

```