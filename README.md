# copilot-cli-0.0.370-6
copilot-cli
import React, { useState, useEffect } from 'react';
import { Code, GitBranch, GitCommit, GitPullRequest, Download, Zap, Trophy, Target, Users, Star, Rocket, Award, Plus, ArrowRight, Check, Activity } from 'lucide-react';

const GitHubCodeGenerator = () => {
  const [step, setStep] = useState(1);
  const [projectData, setProjectData] = useState({
    name: '',
    description: '',
    language: '',
    framework: '',
    features: ''
  });
  const [repoData, setRepoData] = useState({
    repoName: '',
    branch: 'main',
    visibility: 'public'
  });
  const [generatedCode, setGeneratedCode] = useState(null);
  const [loading, setLoading] = useState(false);
  const [userStats, setUserStats] = useState({
    level: 1,
    xp: 0,
    repos: 0,
    commits: 0,
    prs: 0
  });
  const [achievements, setAchievements] = useState([]);
  const [showSuccess, setShowSuccess] = useState(false);

  const languages = ['JavaScript', 'Python', 'Java', 'C#', 'Go', 'Rust', 'TypeScript', 'PHP', 'Ruby', 'Swift'];
  const frameworks = {
    'JavaScript': ['React', 'Vue', 'Angular', 'Node.js', 'Express'],
    'TypeScript': ['React', 'Next.js', 'NestJS', 'Angular'],
    'Python': ['Django', 'Flask', 'FastAPI', 'Streamlit'],
    'Java': ['Spring Boot', 'Quarkus', 'Micronaut'],
    'C#': ['.NET Core', 'ASP.NET', 'Blazor'],
    'Go': ['Gin', 'Echo', 'Fiber'],
    'Rust': ['Actix', 'Rocket', 'Axum'],
    'PHP': ['Laravel', 'Symfony', 'CodeIgniter'],
    'Ruby': ['Rails', 'Sinatra'],
    'Swift': ['SwiftUI', 'UIKit']
  };

  const achievementsList = [
    { id: 'first_repo', name: 'First Repository', icon: '🎯', requirement: 1, type: 'repos' },
    { id: 'code_master', name: 'Code Master', icon: '👨‍💻', requirement: 5, type: 'repos' },
    { id: 'commit_warrior', name: 'Commit Warrior', icon: '⚔️', requirement: 10, type: 'commits' },
    { id: 'pr_expert', name: 'PR Expert', icon: '🔀', requirement: 5, type: 'prs' },
    { id: 'level_5', name: 'Level 5 Developer', icon: '⭐', requirement: 5, type: 'level' }
  ];

  useEffect(() => {
    checkAchievements();
  }, [userStats]);

  const checkAchievements = () => {
    const newAchievements = [];
    achievementsList.forEach(ach => {
      if (!achievements.find(a => a.id === ach.id)) {
        const stat = userStats[ach.type];
        if (stat >= ach.requirement) {
          newAchievements.push(ach);
        }
      }
    });
    if (newAchievements.length > 0) {
      setAchievements([...achievements, ...newAchievements]);
    }
  };

  const addXP = (amount) => {
    const newXP = userStats.xp + amount;
    const newLevel = Math.floor(newXP / 100) + 1;
    setUserStats({ ...userStats, xp: newXP, level: newLevel });
  };

  const generateCode = async () => {
    setLoading(true);
    
    // Симуляция генерации кода с AI
    setTimeout(() => {
      const mockCode = {
        files: [
          {
            name: `${projectData.language === 'JavaScript' || projectData.language === 'TypeScript' ? 'app.js' : 'main.' + projectData.language.toLowerCase()}`,
            content: `// ${projectData.name}\n// ${projectData.description}\n\n// Generated with ${projectData.framework || projectData.language}\n\nfunction main() {\n  console.log("Hello from ${projectData.name}!");\n  // Your code here\n}\n\nmain();`
          },
          {
            name: 'README.md',
            content: `# ${projectData.name}\n\n${projectData.description}\n\n## Tech Stack\n- Language: ${projectData.language}\n${projectData.framework ? `- Framework: ${projectData.framework}` : ''}\n\n## Features\n${projectData.features}\n\n## Installation\n\`\`\`bash\nnpm install\n\`\`\`\n\n## Usage\n\`\`\`bash\nnpm start\n\`\`\``
          },
          {
            name: 'package.json',
            content: `{\n  "name": "${projectData.name.toLowerCase().replace(/\\s+/g, '-')}",\n  "version": "1.0.0",\n  "description": "${projectData.description}",\n  "main": "index.js",\n  "scripts": {\n    "start": "node app.js"\n  }\n}`
          }
        ],
        structure: `${projectData.name}/\n├── app.js\n├── README.md\n├── package.json\n└── .gitignore`
      };
      
      setGeneratedCode(mockCode);
      addXP(50);
      setLoading(false);
      setStep(2);
    }, 2000);
  };

  const createRepo = () => {
    setLoading(true);
    setTimeout(() => {
      setUserStats({
        ...userStats,
        repos: userStats.repos + 1,
        commits: userStats.commits + 1
      });
      addXP(100);
      setShowSuccess(true);
      setLoading(false);
      setTimeout(() => setShowSuccess(false), 3000);
    }, 1500);
  };

  const createCommit = () => {
    setUserStats({ ...userStats, commits: userStats.commits + 1 });
    addXP(20);
    setShowSuccess(true);
    setTimeout(() => setShowSuccess(false), 2000);
  };

  const createPR = () => {
    setUserStats({ ...userStats, prs: userStats.prs + 1 });
    addXP(75);
    setShowSuccess(true);
    setTimeout(() => setShowSuccess(false), 2000);
  };

  const exportCode = () => {
    addXP(30);
    alert('Код экспортирован как ZIP файл! 📦');
  };

  const resetProject = () => {
    setStep(1);
    setProjectData({
      name: '',
      description: '',
      language: '',
      framework: '',
      features: ''
    });
    setGeneratedCode(null);
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-gray-900 via-purple-900 to-gray-900 text-white p-6">
      {/* Header with Stats */}
      <div className="max-w-7xl mx-auto mb-8">
        <div className="flex justify-between items-center mb-6">
          <div className="flex items-center gap-3">
            <div className="bg-purple-600 p-3 rounded-xl">
              <Code size={32} />
            </div>
            <div>
              <h1 className="text-3xl font-bold">GitHub Code Generator Pro</h1>
              <p className="text-gray-400">Создавайте код с AI и управляйте GitHub</p>
            </div>
          </div>
          
          <div className="flex gap-4">
            <div className="bg-gray-800 rounded-xl p-4 min-w-[120px]">
              <div className="flex items-center gap-2 mb-2">
                <Trophy className="text-yellow-400" size={20} />
                <span className="text-sm text-gray-400">Уровень</span>
              </div>
              <div className="text-2xl font-bold">{userStats.level}</div>
              <div className="w-full bg-gray-700 rounded-full h-2 mt-2">
                <div 
                  className="bg-yellow-400 h-2 rounded-full transition-all"
                  style={{ width: `${(userStats.xp % 100)}%` }}
                />
              </div>
            </div>
            
            <div className="bg-gray-800 rounded-xl p-4 min-w-[120px]">
              <div className="flex items-center gap-2 mb-2">
                <Zap className="text-blue-400" size={20} />
                <span className="text-sm text-gray-400">XP</span>
              </div>
              <div className="text-2xl font-bold">{userStats.xp}</div>
            </div>
          </div>
        </div>

        {/* Stats Bar */}
        <div className="grid grid-cols-4 gap-4 mb-6">
          <div className="bg-gray-800 rounded-lg p-4">
            <div className="flex items-center gap-2 text-green-400 mb-2">
              <GitBranch size={18} />
              <span className="text-sm">Репозитории</span>
            </div>
            <div className="text-2xl font-bold">{userStats.repos}</div>
          </div>
          
          <div className="bg-gray-800 rounded-lg p-4">
            <div className="flex items-center gap-2 text-blue-400 mb-2">
              <GitCommit size={18} />
              <span className="text-sm">Коммиты</span>
            </div>
            <div className="text-2xl font-bold">{userStats.commits}</div>
          </div>
          
          <div className="bg-gray-800 rounded-lg p-4">
            <div className="flex items-center gap-2 text-purple-400 mb-2">
              <GitPullRequest size={18} />
              <span className="text-sm">Pull Requests</span>
            </div>
            <div className="text-2xl font-bold">{userStats.prs}</div>
          </div>
          
          <div className="bg-gray-800 rounded-lg p-4">
            <div className="flex items-center gap-2 text-yellow-400 mb-2">
              <Award size={18} />
              <span className="text-sm">Достижения</span>
            </div>
            <div className="text-2xl font-bold">{achievements.length}/{achievementsList.length}</div>
          </div>
        </div>

        {/* Achievements */}
        {achievements.length > 0 && (
          <div className="bg-gray-800 rounded-xl p-4 mb-6">
            <h3 className="font-semibold mb-3 flex items-center gap-2">
              <Trophy className="text-yellow-400" size={20} />
              Достижения
            </h3>
            <div className="flex gap-2 flex-wrap">
              {achievements.map(ach => (
                <div key={ach.id} className="bg-gray-700 rounded-lg px-3 py-2 flex items-center gap-2">
                  <span className="text-2xl">{ach.icon}</span>
                  <span className="text-sm">{ach.name}</span>
                </div>
              ))}
            </div>
          </div>
        )}
      </div>

      {/* Success Notification */}
      {showSuccess && (
        <div className="fixed top-4 right-4 bg-green-600 text-white px-6 py-3 rounded-lg shadow-lg flex items-center gap-2 animate-bounce">
          <Check size={20} />
          <span>Успешно выполнено! +XP</span>
        </div>
      )}

      {/* Main Content */}
      <div className="max-w-7xl mx-auto">
        {step === 1 && (
          <div className="bg-gray-800 rounded-xl p-8">
            <h2 className="text-2xl font-bold mb-6 flex items-center gap-2">
              <Rocket className="text-purple-400" />
              Шаг 1: Опишите ваш проект
            </h2>
            
            <div className="space-y-4">
              <div>
                <label className="block text-sm font-medium mb-2">Название проекта</label>
                <input
                  type="text"
                  value={projectData.name}
                  onChange={(e) => setProjectData({ ...projectData, name: e.target.value })}
                  className="w-full bg-gray-700 border border-gray-600 rounded-lg px-4 py-3 focus:outline-none focus:border-purple-500"
                  placeholder="Мой крутой проект"
                />
              </div>
              
              <div>
                <label className="block text-sm font-medium mb-2">Описание</label>
                <textarea
                  value={projectData.description}
                  onChange={(e) => setProjectData({ ...projectData, description: e.target.value })}
                  className="w-full bg-gray-700 border border-gray-600 rounded-lg px-4 py-3 focus:outline-none focus:border-purple-500 h-24"
                  placeholder="Опишите что должен делать проект..."
                />
              </div>
              
              <div className="grid grid-cols-2 gap-4">
                <div>
                  <label className="block text-sm font-medium mb-2">Язык программирования</label>
                  <select
                    value={projectData.language}
                    onChange={(e) => setProjectData({ ...projectData, language: e.target.value, framework: '' })}
                    className="w-full bg-gray-700 border border-gray-600 rounded-lg px-4 py-3 focus:outline-none focus:border-purple-500"
                  >
                    <option value="">Выберите язык</option>
                    {languages.map(lang => (
                      <option key={lang} value={lang}>{lang}</option>
                    ))}
                  </select>
                </div>
                
                <div>
                  <label className="block text-sm font-medium mb-2">Фреймворк (опционально)</label>
                  <select
                    value={projectData.framework}
                    onChange={(e) => setProjectData({ ...projectData, framework: e.target.value })}
                    className="w-full bg-gray-700 border border-gray-600 rounded-lg px-4 py-3 focus:outline-none focus:border-purple-500"
                    disabled={!projectData.language}
                  >
                    <option value="">Без фреймворка</option>
                    {projectData.language && frameworks[projectData.language]?.map(fw => (
                      <option key={fw} value={fw}>{fw}</option>
                    ))}
                  </select>
                </div>
              </div>
              
              <div>
                <label className="block text-sm font-medium mb-2">Основные функции</label>
                <textarea
                  value={projectData.features}
                  onChange={(e) => setProjectData({ ...projectData, features: e.target.value })}
                  className="w-full bg-gray-700 border border-gray-600 rounded-lg px-4 py-3 focus:outline-none focus:border-purple-500 h-24"
                  placeholder="- Аутентификация пользователей&#10;- REST API&#10;- База данных"
                />
              </div>
              
              <button
                onClick={generateCode}
                disabled={!projectData.name || !projectData.language || loading}
                className="w-full bg-purple-600 hover:bg-purple-700 disabled:bg-gray-600 disabled:cursor-not-allowed text-white font-semibold py-4 rounded-lg flex items-center justify-center gap-2 transition-colors"
              >
                {loading ? (
                  <>
                    <Activity className="animate-spin" size={20} />
                    Генерация кода...
                  </>
                ) : (
                  <>
                    <Zap size={20} />
                    Сгенерировать код с AI
                    <ArrowRight size={20} />
                  </>
                )}
              </button>
            </div>
          </div>
        )}

        {step === 2 && generatedCode && (
          <div className="space-y-6">
            <div className="bg-gray-800 rounded-xl p-8">
              <h2 className="text-2xl font-bold mb-6 flex items-center gap-2">
                <Code className="text-green-400" />
                Шаг 2: Сгенерированный код
              </h2>
              
              <div className="bg-gray-900 rounded-lg p-4 mb-4">
                <h3 className="text-sm font-semibold mb-2 text-gray-400">Структура проекта:</h3>
                <pre className="text-green-400 text-sm">{generatedCode.structure}</pre>
              </div>
              
              <div className="space-y-3 mb-6">
                {generatedCode.files.map((file, idx) => (
                  <details key={idx} className="bg-gray-900 rounded-lg">
                    <summary className="cursor-pointer p-4 font-medium hover:bg-gray-800 rounded-lg">
                      📄 {file.name}
                    </summary>
                    <pre className="p-4 text-sm overflow-x-auto text-gray-300">{file.content}</pre>
                  </details>
                ))}
              </div>
              
              <div className="bg-gray-700 rounded-lg p-6">
                <h3 className="font-semibold mb-4 flex items-center gap-2">
                  <GitBranch className="text-purple-400" />
                  Настройки репозитория
                </h3>
                
                <div className="space-y-4">
                  <div>
                    <label className="block text-sm font-medium mb-2">Название репозитория</label>
                    <input
                      type="text"
                      value={repoData.repoName}
                      onChange={(e) => setRepoData({ ...repoData, repoName: e.target.value })}
                      className="w-full bg-gray-600 border border-gray-500 rounded-lg px-4 py-2 focus:outline-none focus:border-purple-500"
                      placeholder={projectData.name.toLowerCase().replace(/\s+/g, '-')}
                    />
                  </div>
                  
                  <div className="grid grid-cols-2 gap-4">
                    <div>
                      <label className="block text-sm font-medium mb-2">Ветка</label>
                      <input
                        type="text"
                        value={repoData.branch}
                        onChange={(e) => setRepoData({ ...repoData, branch: e.target.value })}
                        className="w-full bg-gray-600 border border-gray-500 rounded-lg px-4 py-2 focus:outline-none focus:border-purple-500"
                      />
                    </div>
                    
                    <div>
                      <label className="block text-sm font-medium mb-2">Видимость</label>
                      <select
                        value={repoData.visibility}
                        onChange={(e) => setRepoData({ ...repoData, visibility: e.target.value })}
                        className="w-full bg-gray-600 border border-gray-500 rounded-lg px-4 py-2 focus:outline-none focus:border-purple-500"
                      >
                        <option value="public">Public</option>
                        <option value="private">Private</option>
                      </select>
                    </div>
                  </div>
                </div>
              </div>
              
              <div className="grid grid-cols-2 gap-4 mt-6">
                <button
                  onClick={createRepo}
                  disabled={loading}
                  className="bg-green-600 hover:bg-green-700 disabled:bg-gray-600 text-white font-semibold py-3 rounded-lg flex items-center justify-center gap-2 transition-colors"
                >
                  <Plus size={20} />
                  Создать репозиторий
                </button>
                
                <button
                  onClick={exportCode}
                  className="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 rounded-lg flex items-center justify-center gap-2 transition-colors"
                >
                  <Download size={20} />
                  Экспортировать ZIP
                </button>
              </div>
            </div>

            <div className="bg-gray-800 rounded-xl p-8">
              <h3 className="text-xl font-bold mb-4 flex items-center gap-2">
                <Activity className="text-blue-400" />
                Управление репозиторием
              </h3>
              
              <div className="grid grid-cols-3 gap-4">
                <button
                  onClick={createCommit}
                  className="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 rounded-lg flex items-center justify-center gap-2 transition-colors"
                >
                  <GitCommit size={20} />
                  Создать коммит
                </button>
                
                <button
                  onClick={createPR}
                  className="bg-purple-600 hover:bg-purple-700 text-white font-semibold py-3 rounded-lg flex items-center justify-center gap-2 transition-colors"
                >
                  <GitPullRequest size={20} />
                  Создать PR
                </button>
                
                <button
                  onClick={resetProject}
                  className="bg-gray-600 hover:bg-gray-700 text-white font-semibold py-3 rounded-lg flex items-center justify-center gap-2 transition-colors"
                >
                  <Plus size={20} />
                  Новый проект
                </button>
              </div>
            </div>
          </div>
        )}
      </div>
    </div>
  );
};

export default GitHubCodeGenerator;
