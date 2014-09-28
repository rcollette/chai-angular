# chai-angular

chai-angular is a collection of small utilities that help testing [Angular](https://angularjs.org/) apps with [Chai](http://chaijs.com/).

The collection is quite modest for now, since it only contains one utility. Hopefully it will remain that small — the more things work out of the box between Angular and Chai, the better!

## resourceEql

resourceEql compares Angular [resources](https://docs.angularjs.org/api/ngResource/service/$resource) with plain objects.

Let's say the Angular controller `mycontroller` contains this code:

```javascript
var User = $resource('/user/:id');
$scope.user = User.get({id: 9});
```

A typical test for this controller would be:

```javascript
var $scope = {};

$httpBackend.expectGET('/user/9').respond({id: '9', name: 'Roger Zelazny'});
$controller('mycontroller', {$scope: $scope});
$httpBackend.flush();

$scope.user.should.deep.equal({id: '9', name: 'Roger Zelazny'});
```

However, this test fails because `$scope.user` is not a plain old Javascript object, but an Angular resource, and Chai's deep equality sees them as different.

To make the test pass, install the chai-angular plugin, then replace `deep.equal` with `deep.resource.equal`:

```javascript
$scope.user.should.deep.resource.equal({id: '9', name: 'Roger Zelazny'});
```

The usual Chai syntactic variants are also available:

```javascript
$scope.user.should.resourceEql({id: '9', name: 'Roger Zelazny'});
expect($scope.user).to.deep.resource.equal({id: '9', name: 'Roger Zelazny'});
expect($scope.user).to.resourceEql({id: '9', name: 'Roger Zelazny'});
```

## Installation

```shell
$ npm install chai-angular --save-dev
```

## Usage with [Karma](http://karma-runner.github.io)

Simply add the path to `chai-angular.js` to the `files` in `karma.conf.js`:

```javascript
module.exports = function(config) {
    config.set({
        basePath : '../',
        files : [
            'node_modules/chai_angular/chai-angular.js',
            ...
        ],
        ...
    });
};
```

Then you can immediately start using chai-angular assertions in your tests.
