Date: 2013-06-26
Title: Overriding Jasmine Spy Returns in Nested Describe Blocks

Let's say you have the following Jasmine unit tests for a trivial `personService`:

    describe('PersonService', function() {
      var mockPeople = ['Clive', 'Terry', 'Kate', 'Superman'];
    
      beforeEach(function() {
        spyOn(personService, 'getPeople').andReturn(mockPeople);
      });
    
      describe('getPeopleCount()', function() {
        it('should have 4 people in the list', function() {
          expect(personService.getPeople().length).toEqual(4);
        });

        it('should have Clive as the first person', function() {
          expect(personService.getPeople()[0]).toBe('Clive');
        });
      });
    });

Simple enough. Mock an array of people and return it, do some simple checks. 

Important to note here that I am creating a mock object (instead of returning the array directly) because I often want to re-use the object for different reasons.  For example if I had two lists of people, an original list when the service was created and a new list to track changes. By using a mock object I can add many spies without repeating the actual dataset which in some cases can be quite large. In this trivial example it seems silly, but I digress.

Now we want to add a simple check to make sure the count works after the list of people is updated.

    describe('PersonService', function() {
      var mockPeople = ['Clive', 'Terry', 'Kate', 'Superman'];
    
      beforeEach(function() {
        spyOn(personService, 'getPeople').andReturn(mockPeople);
      });
    
      describe('getPeopleCount()', function() {
        it('should have 4 people in the list', function() {
          expect(personService.getPeople().length).toEqual(4);
        });

        it('should have Clive as the first person', function() {
          expect(personService.getPeople()[0]).toBe('Clive');
        });

        it('should have 5 people after a person is added', function() {
          mockPeople.push('Batman');
          expect(personService.getPeople().length).toEqual(5);
        });
      });
    });

As you can see we have added a new test that appends a person to the array and checks the result again.

If you re-run the entire suite, the **first test will fail**!  There will actually be 5 people in the list, not 4. This is because the line `mockPeople.push('Batman')` is actually run before any of the tests.  So right from the start your array of people will have Batman in it and a length of 5.

So, how do we fix this?  A ridiculously easy solution is to re-create the spy's return object for any tests that require changes to the initial mock data.

    it('should return 5 after a person is added', function() {
      personService.getPeople
        .andReturn(['Clive', 'Terry', 'Kate', 'Superman', 'Batman']);

      expect(personService.getPeople().length).toEqual(5);
    });

And there you go, everything will now pass.  Happy testing!