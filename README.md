# passage-analyser

    Chinese word segmentation implementation based in node.js

# installation

      npm i passage-analyser

# core section

    Segmenter.js : main entry which calls subordinate segmenters including:

    LetterSegmenter.js: handler for english and arabic number chars

    CN_QuantifierSegmenter.js: handler for chinese quantifiers

    CJKSegmenter.js: handler for common characters in Chinese/Japanese/Korean

    IKArbitrator.js: similarity handler (Idea comes from GitHub program IKAnalyzer)

    AnalyzeContext.js: analyze the context to handle possible merge or change of property of the current Hit words

#TEST SUITE(From SITRAN Bakeoff 2005)

    A set of unit tests are implemented in the test.js.
    A recognized Chinese text segmentation dataset and its training model. Prepared for further instrumented tests(Based on the comparison between analyzer output and standard golden output provided by SITRAN). Does not matter in Audit 1.

# Why a decent segmentation is important in Chinese context

    One important feature of Chinese texts is that they are character-based, not word- based. Each Chinese character stands for one phonological syllable and in most cases represents a morpheme. The fact that Chinese writing does not mark word boundaries poses the unique question of word segmentation in Chinese computational linguistics (e.g. Sproat and Shih 1990, and Chert and Liu 1992). Since words are the linguistically significant basic elements that are entered in the lexicon and manipulated by grammar rules, no language processing can be done unless words, especially those compound words such as "中国人民银行"(which can be segmented into "中国/人民银行"(The People's Bank Of China), or "中国人民/银行"(Chinese folks and banks)) are identified.

    Example: Chinese text = ' 羽毛球拍卖完了';
             Correct segmentation = '羽毛球拍/卖完/了'
             Translation to English = 'The badminton rackets are sold out'
             Incorrect segmentation = '羽毛球/拍卖/完了'
             Translation to English = 'The auction of badminton is finished'


# How to use

% node test.js

      var Segmenter = require('./Segmenter');
      var segmenter = new Segmenter();

      var txt = '......';
      console.log('txt: ', txt);

      var result = segmenter.analyze(txt);
      console.log('result: ', result);

      // test cases:
      txt:   Raheem Sterlings volley salvaged a point for 10-man Manchester City as Everton were denied victory on the night Wayne Rooney scored his 200th Premier League goal.
      result:    Raheem   Sterlings   volley   salvaged   a   point   for   10 - man   Manchester   City   as   Everton   were   denied   victory   on   the   night   Wayne   Rooney   scored   his   200th   Premier   League   goal .
      txt:   上海自来水来自海上
      result:    上海 自来水 来自 海上
      txt:   白化病患者
      result:    白化病 患者
      txt:   x射线检查由B超组代为实施
      result:    x 射线 检查 由 B 超 组 代为 实施
      txt:   乒乓球拍卖完了
      result:    乒乓球拍 卖完 了
      txt:   cersei.Lannister@gmail.com
      result:    cersei . Lannister @ gmail .

# Dictionaries

Default Dictionary location:  ./lib/dict，to activate a customised dic: // 全部字段可选

      var opts = {
        MainDictPath: 'your_dict_folder/main.dic',
        SurnameDictPath: 'your_dict_folder/dict/surname.dic',
        QuantifierDictPath: 'your_dict_folder/dict/quantifier.dic',
        SuffixDictPath: 'your_dict_folder/dict/suffix.dic',
        PrepDictPath: 'your_dict_folder/dict/preposition.dic',
        StopWordDictPath: 'your_dict_folder/dict/stopword.dic',
        ExtraDictConfig: {
          ext_dict: [],
          ext_stopwords: []
        }
      };
      var segmenter = new Segmenter(opts);

      // var segmenter = new Segmenter();   // use Default Dictionaries



# Performance

     initialization time：1094ms
     segmentation speed：--- words/s, --- bytes/s, word count：---，total running time：---ms

# Reference

    IKArbitrator.js & AnalyzeContext.js idea/algorithm from (open-sourced project) https://code.google.com/archive/p/ik-analyzer/source/default/source
    Project artifact references GitHub open-sourced project from Wang, Li et al: https://github.com/newebug/node-analyzer
