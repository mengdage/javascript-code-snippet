```javascript
function memoize(method) {
  const cache = {}
  return function () {
    const args = JSON.stringify(arguments)
    cache[args] = cache[args] || method.apply(this, arguments)
    return cache[args]
  }
}

let getSoupRecipe = memoize(function(soupType) {
    return http.get(`/api/soup/${soupType}`);
});

let buySoupPan = memoize(async function() {
    return http.get(`/api/soupPan`);
});

let hireSoupChef = memoize(function(soupType) {
    let soupRecipe = getSoupRecipe(soupType)

    return await http.post(`/api/soupChef/hire`, {
        requiredSkills: soupRecipe.requiredSkills
    });
});

let makeSoup = memoize(async function(soupType) {

    let [ soupRecipe, soupPan, soupChef ] = await* [     
      getSoupRecipe(soupType), buySoupPan(), hireSoupChef(soupType)
    ];

    return await http.post(`api/makeSoup`, {
      soupRecipe, soupPan, soupChef
    });
});
```
