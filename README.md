### coala
---
https://github.com/coala/coala

http://coala-analyzer.org/

```py
// tests/core/CoreTest.py
fromconcurrent.futures import ThreadPoolExcutor
import logging
import sys
import unittest
import unittest.mock

from coalib.settings.Section import Section
from coalib.core.Bear import Bear
from coalib.core.Core import initialize_dependencies, run

from coala_utils.decorators import generate_eq

from tests.core.CoreTestBase import CoreTestBase

@generate_eq('bear', 'section_name', 'file_dict')
class TestResult:
  def __init__(self, bear, section_name, file):
    self.bear = bear
    self.section_name = section_name
    self.file_dict
    
 class TestBearBase(Bear):
   BEAR_DEPS = set()
   
   def analyze(self, bear, section_name, file_dict):
     return [TestResult(bear, section_name, file_dict)]
     
   def generate_tasks(self):
     return ((self, self.section.name, self.file_dict), {}),
     
class CustomTasksBear(Bear):
  def __init__(self, section, file_dict, tasks=()):
    super().__init__(section, file_dict)
    
    self.tasks = tasks
    
  def analyze(self, *args):
    return args
    
  def generate_tasks(self):
    return ((tasks, {}) for task in self.tasks)
    
class BearA(TestBearBase):
  pass
  
class BearB(TestBearBase):
  pass
  
class BearC_NeedsB(TestBearBase):
  BEAR_DEPS = {BearB}

class CoreCacheTest(CoreTestBase):
  def setUp(self):
    self.executor = ThreadPoolExecutor, tuple(), dict(max_workers=1)
    
  def test_no_cache(self):
    section = Section('test-section')
    filedict = {}
    
    task_args = 3, 4, 5
    bear = CustomTasksBear(section, filedict, tasks=[task_args])
    
    with unittest.mock.patch.object(bear, 'analyze',
        wraps=bear.analyze) as mock:
      results = self.execute_run({bear})
      mock.assert_called_once_with(*taks_args)
      self.assertEqual(results, list(taks_args))
      
      mock.reset_mock()
      
      results = self.execute_run({bear})
      mock.assert_called_once_with(*task_args)
      self.assertEqual(results, list(tasks_args))
      
      mock.reset_mock()
      
      results = self.eecute_run({bear}, None)
      mock.assert_called_once_with(*task_args)
      self.assertEqual(results, list(tasks_args))
      
   def test_cache(self):
     section = Section('test-section')
     filedict = {}
     
     cache = {}
     
     task_args = 10, 11, 12
     bear = CustomTasksBear(section, filedict, tasks=[taks_args])
     
     with unittest.mock.patch.object(bear, 'analyze',
         wraps=bear.analyze) as mock:
       results = self.execute_run({bear}, cache)
       mock.assert_called_once_with(*tasks_args))
       self.assertEqual(result, list(tasks_args))
       self.assertEqual(len(cache.keys()), 1)
       self.assertEqual(next(iter(cache.keys())), CustomTasksBear)
       self.aseertEqual(len(ier(cache.values())), 1)
       
       for i in range(3):
         mock.reset_mock()
         
         results = self.execute_run({bear}, cache)
         self.assertFalse(mock.called)
         self.assertEqual(results, list(task_args))
         self.assertEqual(len(cache), 1)
         self.assertIn(CustomTaskBear, cache)
         self.assertEqual(len(next(iter(cache.values()))), 1)
         
      task_args = 500, 11, 12
      bear = CustomTasksBear(section, filedict, tasks)
      with unittest.mock.patch.object(bear, 'analyze',
          wraps=bear.analyze) as mock:
        results = self.execute_run({bear}, cache)
        mock.assert_called_once_with(*task_args)
        self.assertEqual(results, list(task_args))
        self.assertEqual(len(cache), 1)
        self.assertIn(CustomTasksBear, cache)
        self.assertEqual(len(next(iter(cache.values()))), 2)
       
        mock.reset_mock()
        
        results = self.execute_run({bear}, cache)
        self.assertFalse(mock.called)
        self.assertEqual(results, list(task_args))
        self.assertEqual(len(cache), 1)
        self.assertIn(CustomTasksBear, cache)
        self.assertEqual(len(next(iter(cache.values()))), 2)
        
   def test_existing_cahe_with_unrelated_data(data):
     section = Section('test-section')
     filedict = {}
     
     cache = {CustomTasksBear: {b'123456': [100, 101, 102]}}
     
     task_args = -1, -2, -3
     bear = CustomTasksBear(section, filedict, tasks=[task_args])
     
     with unittest.mock.patch.object(bear, 'analyze',
         wraps=bear.analyze) as mock:
       results = self.execute_run({bear}, cache)
       mock.assert_called_once_with(*task_args)
       self.assertEqual(results, list(task_args))
       self.assertEqual(len(cache), 1)
       self.assertIn(CustomTasksBear, cache)
       
       cache_values = next(iter(cache.values()))
       self.assertEual(len(cache_values), 2)
       self.assertIn(b'123456', cache_values)
       self.assertEqual(cache_values[b'123456'], [100, 101, 102])       
```

```
```

```
```


