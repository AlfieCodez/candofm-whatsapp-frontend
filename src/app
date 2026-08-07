import React, { useState, useEffect } from 'react';
import { Play, Download, LogOut, Mic, Search, Plus, Trash2, Settings } from 'lucide-react';

export default function CANDOFMDashboard() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [loginError, setLoginError] = useState('');
  const [currentUser, setCurrentUser] = useState('');
  const [messages, setMessages] = useState([]);
  const [loading, setLoading] = useState(true);
  const [selectedSender, setSelectedSender] = useState(null);
  const [searchQuery, setSearchQuery] = useState('');
  const [isAdmin, setIsAdmin] = useState(false);
  const [adminTab, setAdminTab] = useState('messages'); // 'messages' or 'accounts'

  // Admin users
  const ADMINS = {
    'hendersona': 'Alfie Henderson',
    'williamsj': 'Jonny Williams',
    'williamsa': 'Andrea Williams'
  };

  const ADMIN_PASSWORD = 'candofm';

  // Mock presenter accounts (in production, fetch from backend)
  const [presenterAccounts, setPresenterAccounts] = useState([
    { id: 1, username: 'testuser', name: 'Test User', permissions: ['view', 'download'] }
  ]);

  const [newPresenter, setNewPresenter] = useState({ username: '', name: '' });

  // Fetch messages from backend
  useEffect(() => {
    if (!isLoggedIn) return;

    const fetchMessages = async () => {
      try {
        const res = await fetch('https://candofm-whatsapp-backend.onrender.com/api/messages');
        const data = await res.json();
        setMessages(data);
        if (data.length > 0 && !selectedSender) {
          setSelectedSender(data[0].from);
        }
      } catch (error) {
        console.error('Failed to load messages:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchMessages();
    const interval = setInterval(fetchMessages, 5000);
    
    return () => clearInterval(interval);
  }, [isLoggedIn]);

  const handleLogin = (e) => {
    e.preventDefault();
    setLoginError('');
    
    if (!username || !password) {
      setLoginError('Username and password required');
      return;
    }
    
    if (!ADMINS[username]) {
      setLoginError('Username not recognized');
      return;
    }

    if (password !== ADMIN_PASSWORD) {
      setLoginError('Incorrect password');
      return;
    }

    setIsLoggedIn(true);
    setCurrentUser(ADMINS[username]);
    setIsAdmin(true); // All logged in users are admins for now
  };

  const handleLogout = () => {
    setIsLoggedIn(false);
    setCurrentUser('');
    setSelectedSender(null);
    setIsAdmin(false);
  };

  const handleDownload = (id) => {
    window.location.href = `https://candofm-whatsapp-backend.onrender.com/api/download/${id}`;
  };

  const addPresenter = () => {
    if (!newPresenter.username || !newPresenter.name) {
      alert('Fill in all fields');
      return;
    }
    
    setPresenterAccounts([
      ...presenterAccounts,
      {
        id: Date.now(),
        username: newPresenter.username,
        name: newPresenter.name,
        permissions: ['view', 'download']
      }
    ]);
    setNewPresenter({ username: '', name: '' });
  };

  const deletePresenter = (id) => {
    setPresenterAccounts(presenterAccounts.filter(p => p.id !== id));
  };

  const getSendersList = () => {
    const senders = {};
    messages.forEach(msg => {
      if (!senders[msg.from]) {
        senders[msg.from] = [];
      }
      senders[msg.from].push(msg);
    });
    return senders;
  };

  const sendersList = getSendersList();
  const senderNames = Object.keys(sendersList).sort();
  const filteredSenders = senderNames.filter(name =>
    name.toLowerCase().includes(searchQuery.toLowerCase())
  );

  const selectedMessages = selectedSender ? sendersList[selectedSender] || [] : [];

  // Get color for initials circle
  const getInitialsColor = (name) => {
    const colors = [
      'bg-blue-500', 'bg-purple-500', 'bg-pink-500', 'bg-red-500',
      'bg-green-500', 'bg-yellow-500', 'bg-indigo-500', 'bg-cyan-500'
    ];
    let hash = 0;
    for (let i = 0; i < name.length; i++) {
      hash = name.charCodeAt(i) + ((hash << 5) - hash);
    }
    return colors[Math.abs(hash) % colors.length];
  };

  const getInitials = (name) => {
    return name
      .split(' ')
      .map(n => n[0])
      .join('')
      .toUpperCase()
      .slice(0, 2);
  };

  if (!isLoggedIn) {
    return (
      <div className="min-h-screen bg-gradient-to-br from-purple-900 via-slate-900 to-slate-900 flex items-center justify-center p-4">
        <div className="w-full max-w-md">
          <div className="text-center mb-8">
            <div className="w-16 h-16 bg-gradient-to-br from-purple-500 to-blue-500 rounded-full flex items-center justify-center mx-auto mb-4">
              <Mic className="w-8 h-8 text-white" />
            </div>
            <h1 className="text-3xl font-bold text-white">CANDOFM</h1>
            <p className="text-slate-400 text-sm mt-1">Live WhatsApp</p>
            <p className="text-slate-400 text-sm">Presenter Studio</p>
          </div>

          <form onSubmit={handleLogin} className="bg-slate-800 rounded-2xl p-8 space-y-5 border border-slate-700">
            <div>
              <label className="block text-sm font-semibold text-slate-200 mb-2">Username</label>
              <input
                type="text"
                value={username}
                onChange={(e) => setUsername(e.target.value)}
                placeholder="e.g. hendersona"
                className="w-full px-4 py-3 bg-slate-700 border border-slate-600 rounded-xl text-white placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-purple-500"
              />
            </div>

            <div>
              <label className="block text-sm font-semibold text-slate-200 mb-2">Password</label>
              <input
                type="password"
                value={password}
                onChange={(e) => setPassword(e.target.value)}
                className="w-full px-4 py-3 bg-slate-700 border border-slate-600 rounded-xl text-white placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-purple-500"
              />
            </div>

            {loginError && (
              <div className="bg-red-500/20 border border-red-500 text-red-200 text-sm px-4 py-3 rounded-lg">
                {loginError}
              </div>
            )}

            <button
              type="submit"
              className="w-full bg-gradient-to-r from-purple-500 to-blue-500 hover:from-purple-600 hover:to-blue-600 text-white font-bold py-3 rounded-xl transition duration-200"
            >
              Log In
            </button>
          </form>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gradient-to-br from-slate-900 via-slate-800 to-slate-900 flex flex-col">
      {/* Header */}
      <header className="bg-gradient-to-r from-purple-900 to-slate-800 border-b border-slate-700 backdrop-blur">
        <div className="px-6 py-4 flex items-center justify-between">
          <div className="flex items-center gap-3">
            <div className="w-10 h-10 bg-gradient-to-br from-purple-500 to-blue-500 rounded-full flex items-center justify-center">
              <Mic className="w-5 h-5 text-white" />
            </div>
            <div>
              <h1 className="text-white font-bold">CANDOFM</h1>
              <p className="text-slate-400 text-xs">Live WhatsApp Presenter Studio</p>
            </div>
          </div>
          <div className="flex items-center gap-3">
            <div className="text-right text-sm">
              <p className="text-slate-200 font-medium">{currentUser}</p>
            </div>
            <button
              onClick={handleLogout}
              className="p-2 hover:bg-slate-700 text-slate-400 hover:text-red-400 rounded-lg transition"
            >
              <LogOut className="w-5 h-5" />
            </button>
          </div>
        </div>

        {/* Admin Tabs */}
        {isAdmin && (
          <div className="flex border-t border-slate-700 px-6">
            <button
              onClick={() => setAdminTab('messages')}
              className={`py-3 px-6 font-medium text-sm transition ${
                adminTab === 'messages'
                  ? 'text-purple-400 border-b-2 border-purple-400'
                  : 'text-slate-400 hover:text-slate-300'
              }`}
            >
              Messages
            </button>
            <button
              onClick={() => setAdminTab('accounts')}
              className={`py-3 px-6 font-medium text-sm transition ${
                adminTab === 'accounts'
                  ? 'text-purple-400 border-b-2 border-purple-400'
                  : 'text-slate-400 hover:text-slate-300'
              }`}
            >
              <Settings className="w-4 h-4 inline mr-2" />
              Presenter Accounts
            </button>
          </div>
        )}
      </header>

      {/* Content */}
      <div className="flex flex-1 overflow-hidden">
        {adminTab === 'messages' ? (
          <>
            {/* Sidebar - Contributors */}
            <div className="w-80 bg-slate-800/50 border-r border-slate-700 flex flex-col">
              <div className="p-4 border-b border-slate-700">
                <div className="relative">
                  <Search className="absolute left-3 top-3 w-4 h-4 text-slate-500" />
                  <input
                    type="text"
                    placeholder="Search..."
                    value={searchQuery}
                    onChange={(e) => setSearchQuery(e.target.value)}
                    className="w-full pl-10 pr-4 py-2 bg-slate-700 border border-slate-600 rounded-lg text-white placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-purple-500 text-sm"
                  />
                </div>
              </div>

              <div className="flex-1 overflow-y-auto">
                {loading ? (
                  <div className="p-4 text-center text-slate-400 text-sm">Loading...</div>
                ) : filteredSenders.length === 0 ? (
                  <div className="p-4 text-center text-slate-400 text-sm">No contributors</div>
                ) : (
                  filteredSenders.map(senderName => {
                    const lastMsg = sendersList[senderName][0];
                    const isSelected = selectedSender === senderName;
                    
                    return (
                      <button
                        key={senderName}
                        onClick={() => setSelectedSender(senderName)}
                        className={`w-full px-4 py-3 flex items-center gap-3 border-b border-slate-700 transition ${
                          isSelected ? 'bg-purple-500/20' : 'hover:bg-slate-700/30'
                        }`}
                      >
                        <div className={`w-12 h-12 rounded-full flex items-center justify-center text-white font-bold flex-shrink-0 ${getInitialsColor(senderName)}`}>
                          {getInitials(senderName)}
                        </div>
                        <div className="flex-1 text-left min-w-0">
                          <p className="text-white font-medium text-sm">{senderName}</p>
                          <p className="text-slate-400 text-xs truncate">
                            {lastMsg.type === 'voice' ? '🎙️ Voice note' : lastMsg.text}
                          </p>
                        </div>
                        <span className="text-slate-500 text-xs flex-shrink-0">{lastMsg.timestamp}</span>
                      </button>
                    );
                  })
                )}
              </div>
            </div>

            {/* Main Chat */}
            <div className="flex-1 flex flex-col bg-slate-900">
              {selectedSender ? (
                <>
                  <div className="bg-slate-800 border-b border-slate-700 px-6 py-4 flex items-center gap-3">
                    <div className={`w-10 h-10 rounded-full flex items-center justify-center text-white font-bold ${getInitialsColor(selectedSender)}`}>
                      {getInitials(selectedSender)}
                    </div>
                    <div>
                      <h2 className="text-white font-semibold">{selectedSender}</h2>
                      <p className="text-slate-400 text-xs">{selectedMessages.length} message{selectedMessages.length !== 1 ? 's' : ''}</p>
                    </div>
                  </div>

                  <div className="flex-1 overflow-y-auto p-6 space-y-3">
                    {selectedMessages.length === 0 ? (
                      <div className="text-center text-slate-400">No messages</div>
                    ) : (
                      selectedMessages.map(msg => (
                        <div key={msg.id} className="bg-slate-800 rounded-xl p-4 border border-slate-700 max-w-xl">
                          <div className="flex items-center justify-between mb-2">
                            <span className="text-purple-400 text-xs font-semibold">
                              {msg.type === 'voice' ? '🎙️ VOICE' : '💬 TEXT'}
                            </span>
                            <span className="text-slate-500 text-xs">{msg.timestamp}</span>
                          </div>

                          <p className="text-slate-200 text-sm mb-3 leading-relaxed">{msg.text}</p>

                          {msg.type === 'voice' && (
                            <button
                              onClick={() => handleDownload(msg.mediaId)}
                              className="w-full flex items-center justify-center gap-2 bg-purple-600 hover:bg-purple-700 text-white text-sm font-semibold py-2 rounded-lg transition"
                            >
                              <Download className="w-4 h-4" />
                              Download Audio
                            </button>
                          )}
                        </div>
                      ))
                    )}
                  </div>
                </>
              ) : (
                <div className="flex-1 flex items-center justify-center text-slate-400">
                  <p>Select a contributor to view messages</p>
                </div>
              )}
            </div>
          </>
        ) : (
          /* Admin - Accounts Tab */
          <div className="flex-1 overflow-y-auto p-8">
            <div className="max-w-2xl">
              <h2 className="text-white text-2xl font-bold mb-6">Manage Presenter Accounts</h2>

              {/* Add New Presenter */}
              <div className="bg-slate-800 border border-slate-700 rounded-xl p-6 mb-8">
                <h3 className="text-white font-semibold mb-4">Add New Presenter</h3>
                <div className="space-y-3">
                  <input
                    type="text"
                    placeholder="Username (e.g. testuser)"
                    value={newPresenter.username}
                    onChange={(e) => setNewPresenter({ ...newPresenter, username: e.target.value })}
                    className="w-full px-4 py-2 bg-slate-700 border border-slate-600 rounded-lg text-white placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-purple-500"
                  />
                  <input
                    type="text"
                    placeholder="Full Name"
                    value={newPresenter.name}
                    onChange={(e) => setNewPresenter({ ...newPresenter, name: e.target.value })}
                    className="w-full px-4 py-2 bg-slate-700 border border-slate-600 rounded-lg text-white placeholder-slate-500 focus:outline-none focus:ring-2 focus:ring-purple-500"
                  />
                  <button
                    onClick={addPresenter}
                    className="w-full bg-gradient-to-r from-purple-600 to-blue-600 hover:from-purple-700 hover:to-blue-700 text-white font-semibold py-2 rounded-lg transition flex items-center justify-center gap-2"
                  >
                    <Plus className="w-4 h-4" />
                    Create Account
                  </button>
                </div>
              </div>

              {/* Accounts List */}
              <div className="space-y-3">
                <h3 className="text-white font-semibold">Active Accounts ({presenterAccounts.length})</h3>
                {presenterAccounts.map(presenter => (
                  <div key={presenter.id} className="bg-slate-800 border border-slate-700 rounded-lg p-4 flex items-center justify-between">
                    <div>
                      <p className="text-white font-medium">{presenter.name}</p>
                      <p className="text-slate-400 text-sm">@{presenter.username}</p>
                      <p className="text-slate-500 text-xs mt-1">Permissions: {presenter.permissions.join(', ')}</p>
                    </div>
                    <button
                      onClick={() => deletePresenter(presenter.id)}
                      className="p-2 hover:bg-red-500/20 text-red-400 rounded-lg transition"
                    >
                      <Trash2 className="w-5 h-5" />
                    </button>
                  </div>
                ))}
              </div>
            </div>
          </div>
        )}
      </div>
    </div>
  );
}
