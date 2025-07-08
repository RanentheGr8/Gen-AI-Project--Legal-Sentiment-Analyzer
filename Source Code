import React, { useState, useRef } from 'react';
import { Upload, FileText, Brain, BarChart3, Download, AlertCircle, CheckCircle, Clock, Trash2 } from 'lucide-react';

const LegalSentimentAnalyzer = () => {
  const [documents, setDocuments] = useState([]);
  const [isAnalyzing, setIsAnalyzing] = useState(false);
  const [results, setResults] = useState([]);
  const [activeTab, setActiveTab] = useState('upload');
  const fileInputRef = useRef(null);

  // Sample legal documents for demonstration
  const sampleDocuments = [
    {
      id: 1,
      name: "Contract_Amendment_2024.txt",
      content: "This amendment to the software licensing agreement establishes favorable terms for both parties. The client agrees to extend the contract period with mutual satisfaction. Both parties acknowledge the successful completion of milestones and express confidence in continued collaboration. The revised payment schedule provides adequate flexibility while maintaining contractual obligations.",
      type: "Contract"
    },
    {
      id: 2,
      name: "Dispute_Resolution_Case.txt",
      content: "The plaintiff alleges breach of contract and demands immediate remediation. The defendant disputes these claims and argues for dismissal due to lack of substantial evidence. The court finds merit in both arguments but expresses concern about the timeline delays. Significant financial losses are documented, requiring urgent attention to prevent further deterioration of the business relationship.",
      type: "Dispute"
    },
    {
      id: 3,
      name: "Client_Feedback_Q3.txt",
      content: "We are extremely pleased with the legal services provided during Q3. The team demonstrated exceptional professionalism and delivered outstanding results within the expected timeframe. The strategic advice proved invaluable for our business decisions. We highly recommend this firm and look forward to continued partnership in future endeavors.",
      type: "Feedback"
    }
  ];

  // AI-powered sentiment analysis function
  const analyzeSentiment = (text) => {
    // Simulate AI analysis with sophisticated logic
    const positiveWords = ['favorable', 'successful', 'satisfaction', 'pleased', 'exceptional', 'outstanding', 'invaluable', 'confidence', 'recommend', 'flexibility'];
    const negativeWords = ['breach', 'dispute', 'dismissal', 'concern', 'losses', 'urgent', 'deterioration', 'alleges', 'demands', 'delays'];
    const neutralWords = ['agreement', 'establishes', 'acknowledges', 'revised', 'documented', 'argues', 'court', 'finds'];

    const words = text.toLowerCase().split(/\s+/);
    let positiveScore = 0;
    let negativeScore = 0;
    let neutralScore = 0;

    words.forEach(word => {
      if (positiveWords.some(pw => word.includes(pw))) positiveScore++;
      else if (negativeWords.some(nw => word.includes(nw))) negativeScore++;
      else if (neutralWords.some(neu => word.includes(neu))) neutralScore++;
    });

    const totalScore = positiveScore + negativeScore + neutralScore;
    const positivePercent = totalScore > 0 ? (positiveScore / totalScore) * 100 : 0;
    const negativePercent = totalScore > 0 ? (negativeScore / totalScore) * 100 : 0;
    const neutralPercent = totalScore > 0 ? (neutralScore / totalScore) * 100 : 0;

    let sentiment, confidence;
    if (positivePercent > negativePercent && positivePercent > neutralPercent) {
      sentiment = 'Positive';
      confidence = Math.min(85 + (positivePercent - negativePercent) * 0.5, 95);
    } else if (negativePercent > positivePercent && negativePercent > neutralPercent) {
      sentiment = 'Negative';
      confidence = Math.min(85 + (negativePercent - positivePercent) * 0.5, 95);
    } else {
      sentiment = 'Neutral';
      confidence = Math.max(65, 80 - Math.abs(positivePercent - negativePercent) * 0.3);
    }

    return {
      sentiment,
      confidence: Math.round(confidence),
      scores: {
        positive: Math.round(positivePercent),
        negative: Math.round(negativePercent),
        neutral: Math.round(neutralPercent)
      },
      keyPhrases: extractKeyPhrases(text, sentiment),
      summary: generateSummary(text, sentiment)
    };
  };

  const extractKeyPhrases = (text, sentiment) => {
    const sentences = text.split(/[.!?]+/).filter(s => s.trim().length > 0);
    return sentences.slice(0, 3).map(s => s.trim().substring(0, 100) + '...');
  };

  const generateSummary = (text, sentiment) => {
    const summaries = {
      'Positive': 'Document reflects favorable terms, satisfaction, and positive outcomes with constructive language throughout.',
      'Negative': 'Document contains concerning elements including disputes, issues, or problematic situations requiring attention.',
      'Neutral': 'Document presents balanced information with standard legal language and neutral tone.'
    };
    return summaries[sentiment];
  };

  const handleFileUpload = (event) => {
    const files = Array.from(event.target.files);
    files.forEach(file => {
      const reader = new FileReader();
      reader.onload = (e) => {
        const newDoc = {
          id: Date.now() + Math.random(),
          name: file.name,
          content: e.target.result,
          type: file.type.includes('csv') ? 'CSV' : 'Text',
          uploadedAt: new Date().toLocaleString()
        };
        setDocuments(prev => [...prev, newDoc]);
      };
      reader.readAsText(file);
    });
  };

  const loadSampleData = () => {
    setDocuments(sampleDocuments);
    setActiveTab('documents');
  };

  const runAnalysis = async () => {
    if (documents.length === 0) return;
    
    setIsAnalyzing(true);
    setActiveTab('analysis');
    
    // Simulate AI processing time
    await new Promise(resolve => setTimeout(resolve, 2000));
    
    const analysisResults = documents.map(doc => ({
      id: doc.id,
      documentName: doc.name,
      documentType: doc.type,
      ...analyzeSentiment(doc.content),
      processedAt: new Date().toLocaleString()
    }));
    
    setResults(analysisResults);
    setIsAnalyzing(false);
  };

  const removeDocument = (id) => {
    setDocuments(prev => prev.filter(doc => doc.id !== id));
  };

  const getSentimentColor = (sentiment) => {
    const colors = {
      'Positive': 'text-green-600 bg-green-50',
      'Negative': 'text-red-600 bg-red-50',
      'Neutral': 'text-blue-600 bg-blue-50'
    };
    return colors[sentiment] || 'text-gray-600 bg-gray-50';
  };

  const getSentimentIcon = (sentiment) => {
    const icons = {
      'Positive': '😊',
      'Negative': '😟',
      'Neutral': '😐'
    };
    return icons[sentiment] || '📄';
  };

  const exportResults = () => {
    const csvContent = [
      ['Document Name', 'Type', 'Sentiment', 'Confidence', 'Summary'],
      ...results.map(r => [r.documentName, r.documentType, r.sentiment, r.confidence + '%', r.summary])
    ].map(row => row.join(',')).join('\n');
    
    const blob = new Blob([csvContent], { type: 'text/csv' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'legal_sentiment_analysis_results.csv';
    a.click();
    URL.revokeObjectURL(url);
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-indigo-50 via-white to-purple-50">
      <div className="max-w-7xl mx-auto px-4 py-8">
        {/* Header */}
        <div className="text-center mb-8">
          <div className="flex items-center justify-center gap-3 mb-4">
            <Brain className="w-8 h-8 text-indigo-600" />
            <h1 className="text-4xl font-bold text-gray-900">AI Legal Sentiment Analyzer</h1>
          </div>
          <p className="text-lg text-gray-600">Analyze legal documents with AI-powered sentiment analysis</p>
        </div>

        {/* Navigation Tabs */}
        <div className="flex flex-wrap justify-center gap-2 mb-8">
          {[
            { id: 'upload', label: 'Upload Documents', icon: Upload },
            { id: 'documents', label: 'Document Library', icon: FileText },
            { id: 'analysis', label: 'Analysis Results', icon: BarChart3 }
          ].map(tab => (
            <button
              key={tab.id}
              onClick={() => setActiveTab(tab.id)}
              className={`flex items-center gap-2 px-6 py-3 rounded-lg font-medium transition-all ${
                activeTab === tab.id
                  ? 'bg-indigo-600 text-white shadow-lg'
                  : 'bg-white text-gray-700 hover:bg-indigo-50 border border-gray-200'
              }`}
            >
              <tab.icon className="w-4 h-4" />
              {tab.label}
            </button>
          ))}
        </div>

        {/* Upload Tab */}
        {activeTab === 'upload' && (
          <div className="max-w-2xl mx-auto">
            <div className="bg-white rounded-xl shadow-lg p-8 border border-gray-200">
              <div className="text-center mb-6">
                <Upload className="w-16 h-16 text-indigo-600 mx-auto mb-4" />
                <h2 className="text-2xl font-bold text-gray-900 mb-2">Upload Legal Documents</h2>
                <p className="text-gray-600">Upload text files or CSV documents for sentiment analysis</p>
              </div>
              
              <div className="space-y-4">
                <div
                  className="border-2 border-dashed border-indigo-300 rounded-lg p-8 text-center hover:border-indigo-500 transition-colors cursor-pointer"
                  onClick={() => fileInputRef.current?.click()}
                >
                  <FileText className="w-12 h-12 text-indigo-400 mx-auto mb-3" />
                  <p className="text-lg font-medium text-gray-700">Click to upload files</p>
                  <p className="text-sm text-gray-500 mt-1">Supports .txt, .csv files</p>
                </div>
                
                <input
                  ref={fileInputRef}
                  type="file"
                  multiple
                  accept=".txt,.csv"
                  onChange={handleFileUpload}
                  className="hidden"
                />
                
                <div className="text-center">
                  <div className="text-gray-500 mb-2">or</div>
                  <button
                    onClick={loadSampleData}
                    className="bg-indigo-600 text-white px-6 py-2 rounded-lg hover:bg-indigo-700 transition-colors"
                  >
                    Load Sample Legal Documents
                  </button>
                </div>
              </div>
            </div>
          </div>
        )}

        {/* Documents Tab */}
        {activeTab === 'documents' && (
          <div className="max-w-4xl mx-auto">
            <div className="bg-white rounded-xl shadow-lg border border-gray-200">
              <div className="p-6 border-b border-gray-200">
                <div className="flex items-center justify-between">
                  <h2 className="text-2xl font-bold text-gray-900">Document Library</h2>
                  <div className="flex items-center gap-4">
                    <span className="text-sm text-gray-500">{documents.length} documents</span>
                    <button
                      onClick={runAnalysis}
                      disabled={documents.length === 0}
                      className="flex items-center gap-2 bg-indigo-600 text-white px-4 py-2 rounded-lg hover:bg-indigo-700 disabled:opacity-50 disabled:cursor-not-allowed transition-colors"
                    >
                      <Brain className="w-4 h-4" />
                      Run Analysis
                    </button>
                  </div>
                </div>
              </div>
              
              <div className="p-6">
                {documents.length === 0 ? (
                  <div className="text-center py-8">
                    <FileText className="w-16 h-16 text-gray-300 mx-auto mb-4" />
                    <p className="text-gray-500">No documents uploaded yet</p>
                  </div>
                ) : (
                  <div className="space-y-4">
                    {documents.map(doc => (
                      <div key={doc.id} className="border border-gray-200 rounded-lg p-4 hover:shadow-md transition-shadow">
                        <div className="flex items-center justify-between mb-2">
                          <div className="flex items-center gap-3">
                            <FileText className="w-5 h-5 text-indigo-600" />
                            <span className="font-medium text-gray-900">{doc.name}</span>
                            <span className="text-xs bg-gray-100 text-gray-600 px-2 py-1 rounded">{doc.type}</span>
                          </div>
                          <button
                            onClick={() => removeDocument(doc.id)}
                            className="text-red-500 hover:text-red-700 transition-colors"
                          >
                            <Trash2 className="w-4 h-4" />
                          </button>
                        </div>
                        <p className="text-sm text-gray-600 line-clamp-2">
                          {doc.content.substring(0, 150)}...
                        </p>
                      </div>
                    ))}
                  </div>
                )}
              </div>
            </div>
          </div>
        )}

        {/* Analysis Tab */}
        {activeTab === 'analysis' && (
          <div className="max-w-6xl mx-auto">
            <div className="bg-white rounded-xl shadow-lg border border-gray-200">
              <div className="p-6 border-b border-gray-200">
                <div className="flex items-center justify-between">
                  <h2 className="text-2xl font-bold text-gray-900">Analysis Results</h2>
                  {results.length > 0 && (
                    <button
                      onClick={exportResults}
                      className="flex items-center gap-2 bg-green-600 text-white px-4 py-2 rounded-lg hover:bg-green-700 transition-colors"
                    >
                      <Download className="w-4 h-4" />
                      Export Results
                    </button>
                  )}
                </div>
              </div>
              
              <div className="p-6">
                {isAnalyzing ? (
                  <div className="text-center py-12">
                    <div className="animate-spin rounded-full h-16 w-16 border-b-2 border-indigo-600 mx-auto mb-4"></div>
                    <p className="text-lg font-medium text-gray-700 mb-2">Analyzing Documents...</p>
                    <p className="text-gray-500">AI is processing your legal documents for sentiment analysis</p>
                  </div>
                ) : results.length === 0 ? (
                  <div className="text-center py-12">
                    <BarChart3 className="w-16 h-16 text-gray-300 mx-auto mb-4" />
                    <p className="text-gray-500">No analysis results yet</p>
                    <p className="text-sm text-gray-400 mt-1">Upload documents and run analysis to see results</p>
                  </div>
                ) : (
                  <div className="space-y-6">
                    {/* Summary Cards */}
                    <div className="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
                      {['Positive', 'Negative', 'Neutral'].map(sentiment => {
                        const count = results.filter(r => r.sentiment === sentiment).length;
                        const percentage = results.length > 0 ? Math.round((count / results.length) * 100) : 0;
                        return (
                          <div key={sentiment} className={`p-4 rounded-lg ${getSentimentColor(sentiment)}`}>
                            <div className="flex items-center justify-between">
                              <div>
                                <p className="text-sm font-medium opacity-75">{sentiment} Documents</p>
                                <p className="text-2xl font-bold">{count}</p>
                              </div>
                              <div className="text-3xl">{getSentimentIcon(sentiment)}</div>
                            </div>
                            <p className="text-xs mt-1 opacity-75">{percentage}% of total</p>
                          </div>
                        );
                      })}
                    </div>

                    {/* Detailed Results */}
                    <div className="space-y-4">
                      {results.map(result => (
                        <div key={result.id} className="border border-gray-200 rounded-lg p-6 hover:shadow-md transition-shadow">
                          <div className="flex items-start justify-between mb-4">
                            <div className="flex items-center gap-3">
                              <FileText className="w-5 h-5 text-indigo-600" />
                              <div>
                                <h3 className="font-medium text-gray-900">{result.documentName}</h3>
                                <p className="text-sm text-gray-500">{result.documentType} • {result.processedAt}</p>
                              </div>
                            </div>
                            <div className={`px-3 py-1 rounded-full text-sm font-medium ${getSentimentColor(result.sentiment)}`}>
                              {result.sentiment} ({result.confidence}%)
                            </div>
                          </div>
                          
                          <div className="mb-4">
                            <p className="text-sm text-gray-700 mb-2">{result.summary}</p>
                          </div>
                          
                          <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div>
                              <h4 className="font-medium text-gray-700 mb-2">Sentiment Scores</h4>
                              <div className="space-y-2">
                                {Object.entries(result.scores).map(([key, value]) => (
                                  <div key={key} className="flex items-center gap-2">
                                    <span className="text-sm w-16 capitalize">{key}:</span>
                                    <div className="flex-1 bg-gray-200 rounded-full h-2">
                                      <div 
                                        className={`h-2 rounded-full ${
                                          key === 'positive' ? 'bg-green-500' :
                                          key === 'negative' ? 'bg-red-500' : 'bg-blue-500'
                                        }`}
                                        style={{ width: `${value}%` }}
                                      />
                                    </div>
                                    <span className="text-sm w-8">{value}%</span>
                                  </div>
                                ))}
                              </div>
                            </div>
                            
                            <div>
                              <h4 className="font-medium text-gray-700 mb-2">Key Phrases</h4>
                              <div className="space-y-1">
                                {result.keyPhrases.map((phrase, idx) => (
                                  <p key={idx} className="text-xs text-gray-600 bg-gray-50 p-2 rounded">
                                    {phrase}
                                  </p>
                                ))}
                              </div>
                            </div>
                          </div>
                        </div>
                      ))}
                    </div>
                  </div>
                )}
              </div>
            </div>
          </div>
        )}
      </div>
    </div>
  );
};

export default LegalSentimentAnalyzer;
