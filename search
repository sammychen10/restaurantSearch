let data = lib220.loadJSONFromURL('https://people.cs.umass.edu/~joydeepb/yelp.json');
// lib220.getProperty(jsonObj, memberStr)
// getProperty(obj: Object, memberStr: string):
//  { found: true, value: any } | { found: false }

// type Restaurant = {
//  name: string,
//  city: string,
//  state: string,
//  stars: number,
//  review_count: number,
//  attributes: {} | {
//  Ambience: {
//  [key: string]: boolean
//  }
//  },
//  categories: string[]
// }

class FluentRestaurants{
  constructor(jsonData){
    this.data = jsonData;
  }
  //fromState(stateStr: string): FluentRestaurants
  fromState(stateStr){
    if(typeof(stateStr) !== 'string'){
      return [];
    }
    let exists = this.data.filter(x => lib220.getProperty(x, 'state').found);
    return new FluentRestaurants(exists.filter(x => lib220.getProperty(x, 'state').value === stateStr));
  }
  //ratingLeq(rating: number): FluentRestaurants
  ratingLeq(rating){
    if(rating < 0){
      return [];
    }
    let exists = this.data.filter(x => lib220.getProperty(x, 'stars').found);
    return new FluentRestaurants(exists.filter(x => lib220.getProperty(x, 'stars').value <= rating));
  }
  //ratingGeq(rating: number): FluentRestaurants
  ratingGeq(rating){
    if(rating < 0){
      return [];
    }
    let exists = this.data.filter(x => lib220.getProperty(x, 'stars').found);
    return new FluentRestaurants(exists.filter(x => lib220.getProperty(x, 'stars').value >= rating));
  }
  //category(categoryStr: string): FluentRestaurants
  category(categoryStr){
    if(typeof(categoryStr) !== 'string'){
      return [];
    }
    let exists = this.data.filter(x => lib220.getProperty(x, 'categories').found);
    return new FluentRestaurants(exists.filter(x => lib220.getProperty(x, 'categories').value.includes(categoryStr)));
  }
  //hasAmbience(ambienceStr: string): FluentRestaurants
  hasAmbience(ambienceStr){
    if(typeof(ambienceStr) !== 'string'){
      return [];
    }
    let filtering = this.data.filter(x => lib220.getProperty((lib220.getProperty(x, 'attributes').value), 'Ambience').found);
    filtering = filtering.filter(x => lib220.getProperty(lib220.getProperty((lib220.getProperty(x, 'attributes').value), 'Ambience').value, ambienceStr).found);
    filtering = filtering.filter(x => lib220.getProperty(lib220.getProperty((lib220.getProperty(x, 'attributes').value), 'Ambience').value, ambienceStr).value);
    return new FluentRestaurants(filtering);
  }
  //bestPlace(): Restaurant | {}
  bestPlace(){
    let check = this.data.filter(x => lib220.getProperty(x, 'stars').found);
    if(check.length > 0){
       let starArr = [];
       let existsStars = this.data.filter(x => lib220.getProperty(x, 'stars').found); 
       existsStars.forEach(x => starArr.push(lib220.getProperty(x, 'stars').value));
       let maxStar = Math.max.apply(Math, starArr);
       let highestRated = existsStars.filter(x => lib220.getProperty(x, 'stars').value === maxStar);
       if(highestRated.length > 1){
          let mostReviewedArr = [];
          let existsReviews = highestRated.filter(x => lib220.getProperty(x, 'review_count').found);
          existsReviews.forEach(x => mostReviewedArr.push(lib220.getProperty(x, 'review_count').value));
          let highestReviewed = Math.max.apply(Math, mostReviewedArr);
          let maxReviewed = existsReviews.filter(x => lib220.getProperty(x, 'review_count').value === highestReviewed);
          return maxReviewed[0];
        }
        return highestRated[0];
      }
      return {};
  }
  //mostReviews(): Restaurant | {}
  mostReviews(){
    let check = this.data.filter(x => lib220.getProperty(x, 'review_count').found);
       if(check.length > 0){ 
          let reviewArr = []; 
          let existsReviews = this.data.filter(x => lib220.getProperty(x, 'review_count').found); 
          existsReviews.forEach(x => reviewArr.push(lib220.getProperty(x, 'review_count').value));
          let maxReviewCt = Math.max.apply(Math, reviewArr);
          let highestReviewCts = existsReviews.filter(x => lib220.getProperty(x, 'review_count').value === maxReviewCt);
             if(highestReviewCts.length > 1){
                let highestStarArr = [];
                let existsStars = highestReviewCts.filter(x => lib220.getProperty(x, 'stars').found);
                existsStars.forEach(x => highestStarArr.push(lib220.getProperty(x, 'stars').value));
                let highestStars = Math.max.apply(Math, highestStarArr);
                let maxStars = existsStars.filter(x => lib220.getProperty(x, 'stars').value === highestStars);
              return maxStars[0];
        }
        return highestReviewCts[0];
      } 
      return {};
  }
}

const testData = [
{
 name: "Applebee's",
 state: "NC",
 stars: 4,
 review_count: 6,
 },
 {
 name: "China Garden",
 state: "NC",
 stars: 4,
 review_count: 10,
 },
 {
 name: "Beach Ventures Roofing",
 state: "AZ",
 stars: 3,
 review_count: 30,
 },
 {
 name: "Alpaul Automobile Wash",
 state: "NC",
 stars: 3,
 review_count: 30,
 }
]

test("Usage for getProperty", function() {
  let obj = { x: 42, y: "hello"};
  assert(lib220.getProperty(obj, 'x').found === true);
  assert(lib220.getProperty(obj, 'x').value === 42);
  assert(lib220.getProperty(obj, 'y').value === "hello");
  assert(lib220.getProperty(obj, 'z').found === false);
});

test('fromState filters correctly', function() {
  let tObj = new FluentRestaurants(testData);
  let list = tObj.fromState('NC').data;
  assert(list.length === 3);
  assert(list[0].name === "Applebee's");
  assert(list[1].name === "China Garden");
  assert(list[2].name === "Alpaul Automobile Wash");
  let numResult = tObj.fromState(5);
  assert(numResult.length === 0);
});

test('bestPlace tie-breaking', function() {
  let tObj = new FluentRestaurants(testData);
  let place = tObj.fromState('NC').bestPlace();
  assert(place.name === 'China Garden');
});

test('mostReviews tie-breaker', function() {
  let testObj = new FluentRestaurants(testData);
  let place = testObj.mostReviews();
  assert(place.name === 'Beach Ventures Roofing');
});

test('fluent design', function()  {
  let f = new FluentRestaurants(data);
  assert(f.ratingLeq(4).ratingGeq(2).category('Restaurants').hasAmbience('romantic').fromState('AZ').bestPlace().name === 'Verona Chophouse');
});

test('ratingLeq works correctly', function(){
  let testObj = new FluentRestaurants(testData);
  let arr = testObj.ratingLeq(4).data;
  assert(arr.length === 4);
  assert(arr[0].name === "Applebee's");
  assert(arr[1].name === "China Garden");
  assert(arr[2].name === "Beach Ventures Roofing");
  assert(arr[3].name === "Alpaul Automobile Wash");
  let arrNeg = testObj.ratingLeq(-1);
  assert(arrNeg.length === 0);
});

test('ratingGeq works correctly', function(){
  let testObj = new FluentRestaurants(testData);
  let arr = testObj.ratingGeq(4).data;
  assert(arr.length === 2);
  assert(arr[0].name === "Applebee's");
  assert(arr[1].name === "China Garden");
  let arrNeg = testObj.ratingGeq(-1);
  assert(arrNeg.length === 0);
});

test('category works correctly', function(){
  let testObj = new FluentRestaurants(data);
  let result = testObj.category('Food').bestPlace().name;
  assert(result === "Carefree Coffee");
  let noResult = testObj.category('no result').data;
  assert(noResult.length === 0);
  let numResult = testObj.category(5);
  assert(numResult.length === 0);
});

test('hasAmbience works', function(){
  let testObj = new FluentRestaurants(data);
  let result = testObj.hasAmbience('casual').bestPlace().name;
  assert(result === "Defalco's Italian Grocery");
  let noResult = testObj.hasAmbience('no result').data;
  assert(noResult.length === 0);
  let numResult = testObj.hasAmbience(5);
  assert(numResult.length === 0);
});


//  let testObj = new FluentRestaurants(data);
//   let arr = testObj.hasAmbience(4)
//   console.log(arr);
