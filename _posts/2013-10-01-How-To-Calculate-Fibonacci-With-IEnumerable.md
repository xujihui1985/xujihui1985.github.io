---
layout: post
title: "Create FibonacciSequence With IEnumerable"
description: ""
category: "Csharp"
tags: [Enumerable]
---
{% include JB/setup %}

	public class FibonacciSequence : IEnumerable<int>
    {
        private readonly int _numberOfValues;

        public FibonacciSequence(int numberOfValues)
        {
            _numberOfValues = numberOfValues;
        }

        public IEnumerator<int> GetEnumerator()
        {
            return new FibonacciEnumerator(_numberOfValues);
        }

        IEnumerator IEnumerable.GetEnumerator()
        {
            return GetEnumerator();
        }
    }

    public class FibonacciEnumerator : IEnumerator<int>
    {
        private readonly int _numberOfValues;
        private int _currentPosition;
        private int _currentTotal;
        private int _previousTotal;

        public FibonacciEnumerator(int numberOfValues)
        {
            _numberOfValues = numberOfValues;
        }

        public void Dispose()
        {
            GC.SuppressFinalize(this);
        }

        public bool MoveNext()
        {
            if (_currentPosition == 0)
            {
                _currentTotal = 1;
            }
            else
            {
                int newTotal = _previousTotal + _currentTotal;
                _previousTotal = _currentTotal;
                _currentTotal = newTotal;
            }
            _currentPosition++;
            return _currentPosition <= _numberOfValues;
        }

        public void Reset()
        {
            _currentPosition = 0;
            _currentTotal = 0;
            _previousTotal = 0;
        }

        public int Current
        {
            get { return _currentTotal; }
        }

        object IEnumerator.Current
        {
            get { return Current; }
        }
    }

	 [Test]
     public void CanForEachEnumerable()
     {
        var fib = new FibonacciSequence(10);
        foreach (var num in fib)
        {
            Console.WriteLine(num);
        }

     }

# Iterate File With IEnumerable

 	public class MP3Locator : IEnumerable<FileInfo>
    {
        private readonly string _startingPath;

        public MP3Locator(string startingPath)
        {
            _startingPath = startingPath;
        }

        public IEnumerator<FileInfo> GetEnumerator()
        {
            return new MP3Enumerator(_startingPath);
        }

        IEnumerator IEnumerable.GetEnumerator()
        {
            return GetEnumerator();
        }
    }

    public class MP3Enumerator : IEnumerator<FileInfo>
    {
        private readonly IEnumerator<string> _fileEnumerator;
        private readonly string _startingPath;

        public MP3Enumerator(string startingPath)
        {
            _startingPath = startingPath;
            IEnumerable<string> file = Directory.EnumerateFiles(_startingPath, "*.*", SearchOption.AllDirectories);
            _fileEnumerator = file.GetEnumerator();
        }

        public void Dispose()
        {
            _fileEnumerator.Dispose();
        }

        public bool MoveNext()
        {
            return _fileEnumerator.MoveNext();
        }

        public void Reset()
        {
            _fileEnumerator.Reset();
        }

        public FileInfo Current
        {
            get { return new FileInfo(_fileEnumerator.Current); }
        }

        object IEnumerator.Current
        {
            get { return Current; }
        }
    }

	[Test]
    public void EnumateDirector()
    {
        var files = new MP3Locator("D:\\LearningVedio");
        foreach (var file in files)
        {
            Console.WriteLine(file.Name);
            Console.WriteLine(file.Extension);
            Console.WriteLine(file.DirectoryName);
        }
    }