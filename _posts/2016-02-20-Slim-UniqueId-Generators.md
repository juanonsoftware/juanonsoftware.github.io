---
layout: post
title: How to generate shorter unique id about 10 characters?
tags: [studying]
---

My application requires a unique id on some entities but it shouldn't be the GUID (it's good but
it's little long).

There are some suggestion on SO http://stackoverflow.com/a/9279005/744845 but it is still long, a little.

And the new generators come!!! There are three ready-to-use ones. All of these are based on the date of the server.
So once generated, it's easy to calculate the date that ID was generated.

Here is a performance comparison bitween three generators and GUID in one second.

![_config.yml](//i.imgur.com/UVeuLn0.png)

##1. ShortHexIdGenerator

This generator is based on DateTime.Now (take only HHmmss part) to get current value (as a number) before converting to Hexa value

```
public class ShortHexIdGenerator : IIdGenerator
    {
        private readonly DateTime _baseDate = new DateTime(2016, 1, 1);
        private readonly IDictionary<long, IList<long>> _cache = new Dictionary<long, IList<long>>();

        public ShortHexIdGenerator()
        {
        }

        public ShortHexIdGenerator(DateTime baseDate)
        {
            if (baseDate < _baseDate)
            {
                throw new ArgumentOutOfRangeException("baseDate", "The base date should be later than 1/1/2016");
            }

            _baseDate = baseDate;
        }

        public string NewId()
        {
            var now = DateTime.Now.ToString("HHmmss");
            var daysDiff = (DateTime.Today - _baseDate).Days;
            var current = long.Parse(string.Format("{0}{1}", daysDiff, now));
            return IdGeneratorHelper.NewId(_cache, current);
        }
    }
```

##2. SlimHexIdGenerator

This generator is based on DateTime.Now (take only HHmmssfff part) to get current value (as a number) before converting to Hexa value

```
public class SlimHexIdGenerator : IIdGenerator
    {
        private readonly DateTime _baseDate = new DateTime(2016, 1, 1);
        private readonly IDictionary<long, IList<long>> _cache = new Dictionary<long, IList<long>>();

        public SlimHexIdGenerator()
        {
        }

        public SlimHexIdGenerator(DateTime baseDate)
        {
            if (baseDate < _baseDate)
            {
                throw new ArgumentOutOfRangeException("baseDate", "The base date should be later than 1/1/2016");
            }

            _baseDate = baseDate;
        }

        public string NewId()
        {
            var now = DateTime.Now.ToString("HHmmssfff");
            var daysDiff = (DateTime.Today - _baseDate).Days;
            var current = long.Parse(string.Format("{0}{1}", daysDiff, now));
            return IdGeneratorHelper.NewId(_cache, current);
        }
    }
```

##3. TimebaseHexIdGenerator

This generator is based on DateTime.Now.Ticks to get current value (as a number) before converting to Hexa value

```
public class TimebaseHexIdGenerator : IIdGenerator
    {
        private readonly IDictionary<long, IList<long>> _cache = new Dictionary<long, IList<long>>();
        private readonly long _baseValue = new DateTime(2016, 1, 1).Ticks;

        public TimebaseHexIdGenerator()
        {
        }

        public TimebaseHexIdGenerator(DateTime baseDate)
        {
            if (baseDate < new DateTime(_baseValue))
            {
                throw new ArgumentOutOfRangeException("baseDate", "The base date should be later than 1/1/2016");
            }

            _baseValue = baseDate.Ticks;
        }

        public string NewId()
        {
            var current = DateTime.Now.Ticks - _baseValue;
            return IdGeneratorHelper.NewId(_cache, current);
        }
    }
```

#4. The main logic

```
static class IdGeneratorHelper
    {
        public static string NewId(IDictionary<long, IList<long>> cache, long current)
        {
            // If all keys are precedent values, we should remove them from the dictionary
            // Otherwise cache contains current in the keys.
            // To ignore duplicated ID(s), we try to increase values in the list on each generation.

            if (cache.Any() && cache.Keys.Max() < current)
            {
                cache.Clear();
            }

            if (!cache.Any())
            {
                cache.Add(current, new List<long>());
            }

            string secondPart;
            if (cache[current].Any())
            {
                var maxValue = cache[current].Max();
                cache[current].Add(maxValue + 1);
                secondPart = maxValue.ToString(CultureInfo.InvariantCulture);
            }
            else
            {
                cache[current].Add(0);
                secondPart = string.Empty;
            }

            var nextValueFormatted = string.Format("{0}{1}", current, secondPart);
            return UInt64.Parse(nextValueFormatted).ToString("X");
        }
    }
```

