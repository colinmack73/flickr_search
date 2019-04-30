#Flickr Angular Search App

The app is to be used to search for pictures on flickr giving the user the option to search for 5, 10 or 20 pictures per page.

The license used was a MIT licence.

Application was developed with Angularjs

Karma, Jasmine and Phantomjs were used for application testing.

Istanbul was used as a test coverage library.

I used PhantomJS as it is an optimal solution for:

Page automation - Access webpages and extract information using the standard DOM API, or with usual libraries like jQuery. Because PhantomJS can load and manipulate a web page, it is perfect to carry out various page automation tasks.

Screen capture - Programmatically capture web contents, including SVG and Canvas. Create web site screenshots with thumbnail preview. Since PhantomJS is using WebKit, a real layout and rendering engine, it can capture a web page as a screenshot. Because PhantomJS can render anything on the web page, it can be used to convert HTML content styled with CSS but also SVG, images and Canvas elements.

Headless website testing - Run functional tests with frameworks such as Jasmine, QUnit, Mocha, WebDriver, etc. One major use case of PhantomJS is headless testing of web applications. It is suitable for general command-line based testing, within a precommit hook, and as part of a continuous integration system.

Network monitoring - Monitor page loading and export as standard HAR files. Automate performance analysis using YSlow and Jenkins. Because PhantomJS permits the inspection of network traffic, it is suitable to build various analysis on the network behaviour and performance.

I used Jasmine as it:

• Supports asynchronous testing.
• Makes use of 'spies' for implementing test doubles.
• Supports testing of front-end code through a front-end extension of Jasmine called Jasmine-jQuery.
• Is easy to read
• It is browser, framework, platform and language independent.
• As well as behaviour-driven development, Jasmine also supports test driven development

I used Karma Conf js as it:

• Provides helpful tools that make it easier to call Jasmine test while writing code.
• It enables the running of source code against real browsers using the CLI.

I used Isatnbul for Javascript Test Coverage as it:

• Is used for code “coverage”, i.e. how well and comprehensively your tests exercise your code base.


I will demonstrate how to test the application using Karma, Phantomjs & Jasmine.

I will use npm to download the necessary libraries. You can find npm terms below.

npm install phantomjs-prebuilt --save-dev
npm install karma-phantomjs-launcher --save-dev
npm install karma-jasmine --save-dev

After downloading Karma,initialise karma with “karma init” to create karma.conf.js.

I will create test->unit->flickrAngularAppUnitSpec.js file to write the test with following code.
describe('Testing Flickr Angular App',function () {
	
    beforeEach(module("flickrApp"));

    describe('',function () {
    var scope, ctrl;

    beforeEach(inject(function($controller,$rootScope,$httpBackend){
         scope = $rootScope.$new();  
         ctrl = $controller('MainCtrl', {$scope:scope}); 
         httpBackend = $httpBackend;
         rootScope =$rootScope;
      }));

      it('should loading work properly' ,function(){
          expect(scope.loading).toBeDefined();      
          expect(scope.loading).toBe(false);
      });

      it('should get request work' , function () {
        var searchField = "London";
          httpBackend.whenJSONP("http://www.flickr.com/services/feeds/photos_public.gne?tags="+searchField+"&format=json&jsoncallback=JSON_CALLBACK").respond({
              items : [{
                "title": "Snapshot",
                "link": "http://www.flickr.com/photos/hensontsai/24801076185/",
                "media": {"m":"http://farm2.staticflickr.com/1663/24801076185_660fccce57_m.jpg"},
                "date_taken": "2016-02-02T13:56:15-08:00",
                "description": " <p><a href=\"http://www.flickr.com/people/hensontsai/\">Henson.Tsai<\/a> posted a photo:<\/p> <p><a href=\"http://www.flickr.com/photos/hensontsai/24801076185/\" title=\"Snapshot\"><img src=\"http://farm2.staticflickr.com/1663/24801076185_660fccce57_m.jpg\" width=\"240\" height=\"147\" alt=\"Snapshot\" /><\/a><\/p> ",
                "published": "2016-02-03T22:17:01Z",
                "author": "nobody@flickr.com (Henson.Tsai)",
                "author_id": "139308246@N05",
                "tags": "boy sun cute london love smile kids warm sony streetphotography 55mm a7 snapchot"
                },
                {
                "title": "Snapshot",
                "link": "http://www.flickr.com/photos/hensontsai/24801076185/",
                "media": {"m":"http://farm2.staticflickr.com/1663/24801076185_660fccce57_m.jpg"},
                "date_taken": "2016-02-02T13:56:15-08:00",
                "description": " <p><a href=\"http://www.flickr.com/people/hensontsai/\">Henson.Tsai<\/a> posted a photo:<\/p> <p><a href=\"http://www.flickr.com/photos/hensontsai/24801076185/\" title=\"Snapshot\"><img src=\"http://farm2.staticflickr.com/1663/24801076185_660fccce57_m.jpg\" width=\"240\" height=\"147\" alt=\"Snapshot\" /><\/a><\/p> ",
                "published": "2016-02-03T22:17:01Z",
                "author": "nobody@flickr.com (Henson.Tsai)",
                "author_id": "139308246@N05",
                "tags": "boy sun cute london love smile kids warm sony streetphotography 55mm a7 snapchot"
                }]
          });
          scope.getData("London");
          httpBackend.flush();
          expect(scope.images.length).toBe(2);
          expect(scope.images[0].title).toBe("Snapshot");
          expect(scope.loading).toBe(false);

      });

      it('should get request error work' , function () {
        var searchField = "London";
          httpBackend.whenJSONP("http://www.flickr.com/services/feeds/photos_public.gne?tags="+searchField+"&format=json&jsoncallback=JSON_CALLBACK").respond(500);
          scope.getData("London");
          httpBackend.flush();
          expect(scope.images).toBe("Request failed");
          expect(scope.loading).toBe(false);
      });

    });

});
Now we can run “karma start karma.conf.js” to run tests. Tests will be kept running.
