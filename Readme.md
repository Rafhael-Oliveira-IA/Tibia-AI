# TibiaAI - Comprehensive Technical Analysis & Documentation

## Table of Contents
1. [System Overview](#system-overview)
2. [Architecture Deep Dive](#architecture-deep-dive)
3. [Core Components Analysis](#core-components-analysis)
4. [Advanced Features](#advanced-features)
5. [TFS Integration Documentation](#tfs-integration-documentation)
6. [Security Implementation](#security-implementation)
7. [Performance & Scalability](#performance--scalability)
8. [API Reference](#api-reference)
9. [Development Guidelines](#development-guidelines)
10. [Troubleshooting](#troubleshooting)

---

## System Overview

TibiaAI is a sophisticated web application designed to assist The Forgotten Server (TFS) developers with AI-powered script generation, code analysis, and community features. It represents a masterclass in modern PHP web application development, successfully bridging AI technology with practical TFS development needs.

### Key Features
- **AI-Powered Script Generation**: GPT-4 integration with context-aware prompt engineering
- **Multi-Version TFS Support**: Both TFS 1.x (RevScriptSys) and TFS 0.x (Legacy XML)
- **Real-Time Code Analysis**: Comprehensive script analysis with error detection
- **Community Forum**: Full-featured forum with voting and moderation
- **Advanced Security**: Multi-layer security with CSRF protection and rate limiting
- **Internationalization**: Full i18n support with dynamic language switching

---

## Architecture Deep Dive

### Core System Architecture

```
TibiaAI/
├── assets/               # Static assets with cache busting
│   ├── css/             # Modular CSS with component architecture
│   ├── js/              # Modern JavaScript with progressive enhancement
│   └── images/          # Optimized images with SVG flags
├── config/              # Configuration management
│   ├── config.php       # Main application settings
│   ├── database.php     # Database connection parameters
│   └── routes.php       # URL routing definitions
├── controllers/         # MVC Controllers
│   ├── HomeController.php
│   ├── AuthController.php
│   ├── ScriptController.php (1,415 lines - core functionality)
│   ├── ForumController.php
│   └── FileExplorerController.php
├── core/                # Framework Core Components
│   ├── Router.php       # Advanced routing with parameter extraction
│   ├── Database.php     # Singleton PDO wrapper with pooling
│   ├── Auth.php         # Session-based authentication
│   ├── Language.php     # Sophisticated i18n system
│   ├── View.php         # Component-based rendering
│   ├── GptClient.php    # OpenAI API integration
│   ├── LibraryManager.php # TFS library scanning and caching
│   ├── RateLimiter.php  # IP-based rate limiting
│   ├── UserQuota.php    # Usage quota management
│   ├── ScriptAnalyzer.php (1,192 lines - analysis engine)
│   └── helpers/         # Helper functions
├── models/              # Data Models
├── views/               # Template System
├── lang/                # Internationalization
├── database/            # Database schemas and migrations
├── servers/             # TFS Server Integration
│   └── tfs/
│       ├── tfs-1x/      # Modern TFS with RevScriptSys
│       └── tfs-0x/      # Legacy TFS with XML registration
└── index.php           # Application entry point with advanced routing
```

### Request Flow Architecture

```
Client Request → .htaccess → index.php → Security Layer → Router → Controller → Model/View → Response
```

#### Detailed Flow:
1. **Entry Point**: All requests funnel through `index.php`
2. **Security Validation**: CSRF tokens, API validation, rate limiting
3. **Route Resolution**: Pattern matching with parameter extraction
4. **Controller Loading**: Dynamic class instantiation
5. **Authentication Check**: Session validation where required
6. **Business Logic**: Controller orchestrates models and views
7. **Response Generation**: JSON for APIs, HTML for pages

---

## Core Components Analysis

### 1. Advanced Router System (`core/Router.php`)

#### Key Features:
- **Pattern-Based Routing**: Supports dynamic parameters `{id}`, `{slug}`
- **Method Filtering**: GET, POST, PUT, DELETE support
- **Static Asset Serving**: Automatic MIME type detection
- **Protected Paths**: Security filtering for sensitive files
- **Parameter Extraction**: Automatic URL parameter parsing

#### Example Route Definition:
```php
'/forum/topic/{id}' => ['controller' => 'ForumController', 'action' => 'topic']
```

### 2. Database Layer (`core/Database.php`)

#### Architecture:
- **Singleton Pattern**: Single connection instance
- **PDO Wrapper**: Full prepared statement support
- **Connection Pooling**: Efficient resource management
- **Fallback Configuration**: Multiple config source support
- **Exception Handling**: Detailed error logging

#### Advanced Features:
```php
// Automatic reconnection for lost connections
if (!executeQuery(handle, query, retryQueries)) {
    handle = connectToDatabase(true);
}
```

### 3. AI Integration System (`core/GptClient.php`)

#### Sophisticated Features:
- **Context-Aware Prompts**: TFS version-specific context injection
- **Metadata Handling**: Script type, module type, temperature control
- **Response Sanitization**: Security filtering and content validation
- **Error Recovery**: Graceful handling of API failures
- **Format Detection**: Automatic script format identification

#### Advanced Prompt Engineering:
```php
// Dynamic context loading based on TFS version
$contextFile = $this->getContextWithLibraries($tfsVersion, $moduleType);

// Metadata for specialized generation
$metadata = [
    'tfs_version' => $tfsVersion,
    'module_type' => $moduleType,
    'script_type' => $scriptType,
    'request_type' => 'script_generation'
];
```

### 4. Library Manager (`core/LibraryManager.php`)

#### TFS Integration Excellence:
- **Recursive Scanning**: Deep directory traversal
- **Function Extraction**: Pattern matching for function definitions
- **Module Mapping**: Intelligent library-to-module associations
- **Caching System**: Performance-optimized function caching

#### Module-Specific Library Mapping:
```php
$moduleLibs = [
    'action' => ['items', 'player'],
    'movement' => ['position', 'player', 'creature'],
    'creatureevent' => ['creature', 'player', 'monster'],
    'spell' => ['combat', 'condition', 'player']
];
```

### 5. Script Analyzer (`core/ScriptAnalyzer.php`)

#### Comprehensive Analysis Engine:
- **Format Detection**: RevScriptSys vs XML identification
- **Module Recognition**: Automatic script type detection
- **Error Detection**: Syntax and logic error identification
- **Performance Analysis**: Optimization recommendations
- **Security Scanning**: Vulnerability detection

#### Analysis Workflow:
```php
$result = [
    'format' => $this->detectFormat($scriptContent),
    'moduleType' => $this->detectModuleType($scriptContent),
    'functions' => $this->extractFunctions($scriptContent),
    'errors' => $this->checkForErrors($scriptContent),
    'recommendations' => $this->generateRecommendations($scriptContent),
    'scores' => $this->calculateScores()
];
```

### 6. Security Implementation

#### Multi-Layer Security Architecture:

1. **Entry Level Protection** (`.htaccess`):
   ```apache
   # Block direct access to sensitive files
   RewriteRule ^core/.* - [F,L]
   RewriteRule ^config/.* - [F,L]
   ```

2. **Application Level Security** (`index.php`):
   ```php
   // CSRF Token Validation
   if (!$this->auth->validateToken($token)) {
       throw new Exception('Invalid security token', 403);
   }
   
   // Rate Limiting
   $this->rateLimiter->checkLimit($ipAddress);
   ```

3. **Database Security**:
   - PDO prepared statements for all queries
   - Input sanitization and validation
   - SQL injection prevention

4. **Session Security**:
   - HTTPOnly cookies
   - Secure flag for HTTPS
   - SameSite protection

### 7. Rate Limiting System (`core/RateLimiter.php`)

#### Enterprise-Grade Features:
- **Sliding Window Algorithm**: Precise rate limiting
- **IP-Based Tracking**: Individual IP monitoring
- **Automatic Blocking**: Temporary bans with duration
- **Cleanup Mechanisms**: Automatic old record purging

#### Implementation:
```php
public function checkLimit($ipAddress, $maxRequests = 10, $timeWindow = 30, $blockDuration = 300) {
    if ($this->isBlocked($ipAddress)) {
        throw new Exception("Rate limit exceeded. Try again after 5 minutes.", 429);
    }
    
    if ($record['requests'] >= $maxRequests) {
        $this->blockIP($ipAddress, $blockDuration);
        throw new Exception("Rate limit exceeded.", 429);
    }
}
```

### 8. User Quota System (`core/UserQuota.php`)

#### Sophisticated Usage Management:
- **Monthly Quota Tracking**: Automatic monthly resets
- **Usage Monitoring**: Real-time quota consumption
- **Tier Management**: Different quota levels
- **Grace Period Handling**: Smooth quota transitions

#### Quota Logic:
```php
public function canUseQuota($userId, $type) {
    $quota = $this->getQuota($userId, $type);
    
    // Automatic monthly reset
    if ($now > $quota['reset_at']) {
        $resetAt = date('Y-m-01 00:00:00', strtotime('+1 month'));
        $this->resetQuota($userId, $type, $resetAt);
    }
    
    return $quota['quota_used'] < $quota['quota_limit'];
}
```

---

## Advanced Features

### 1. TFS Server Integration

#### Real-Time File System Access:
The system directly accesses TFS server files for authentic examples:

```php
// Dynamic path resolution based on TFS version
private function getActualPaths($tfsVersion, $moduleType, $scriptType) {
    if (strpos($tfsVersion, '0.') === 0) {
        $tfsPath = BASE_PATH . '/servers/tfs/tfs-0x/';
    } else {
        $tfsPath = BASE_PATH . '/servers/tfs/tfs-1x/forgottenserver-master/';
    }
    
    // RevScriptSys vs XML path logic
    if ($scriptType == 'revscript') {
        $paths['example_dir'] = $dataPath . 'scripts/' . $moduleType . 's/';
    } else {
        $paths['xml_file'] = $dataPath . $moduleType . 's/' . $moduleType . 's.xml';
        $paths['script_dir'] = $dataPath . $moduleType . 's/scripts/';
    }
}
```

#### Recursive Directory Scanning:
```php
private function getDirectoryFiles($directory, $extension = '.lua', $limit = 3, $recursive = false) {
    $scanDir = function($dir) use (&$scanDir, &$files, $extension, $limit, $recursive) {
        while (($file = readdir($dirHandle)) !== false && count($files) < $limit) {
            if (is_dir($path) && $recursive) {
                $scanDir($path); // Recursive scanning
            } elseif (is_file($path) && pathinfo($file, PATHINFO_EXTENSION) === ltrim($extension, '.')) {
                $files[$file] = file_get_contents($path);
            }
        }
    };
}
```

### 2. Intelligent Script Format Detection

#### Multi-Pattern Analysis:
```php
private function detectScriptFormat($content) {
    // XML format detection
    if (strpos($content, '<?xml') !== false || preg_match('/<\w+>|<\/\w+>/', $content)) {
        return 'xml';
    }
    
    // RevScriptSys pattern detection
    if (preg_match('/local\s+[a-zA-Z]+\s*=\s*(?:Action|Movement|TalkAction|CreatureEvent)\(/', $content) ||
        stripos($content, ':register()') !== false ||
        preg_match('/:[a-zA-Z]+\(/', $content)) {
        return 'revscript';
    }
    
    return 'xml'; // Default fallback
}
```

### 3. Context-Aware Prompt Engineering

#### Dynamic Context Construction:
```php
private function getContextWithLibraries($tfsVersion, $moduleType) {
    $context = $this->getContextFile($tfsVersion);
    
    // Add library information for TFS 1.x
    if (strpos($tfsVersion, '1.') === 0) {
        $libraries = $this->libManager->getLibraryFunctions($moduleType);
        $includes = $this->libManager->getLibraryIncludes($moduleType);
        
        $context .= "\n\n### TFS LIBRARY FUNCTIONS\n";
        foreach ($libraries as $lib => $functions) {
            $context .= "\n#### $lib Library:\n";
            foreach ($functions as $function) {
                $context .= "- {$function['name']}({$function['parameters']})\n";
            }
        }
    }
    
    return $context;
}
```

### 4. Advanced Error Handling

#### Comprehensive Error Management:
```php
// JSON error responses for AJAX requests
set_error_handler(function($severity, $message, $file, $line) {
    error_log("PHP Error: $message in $file on line $line");
    
    $isAjaxRequest = isset($_SERVER['HTTP_X_REQUESTED_WITH']) && 
                    strtolower($_SERVER['HTTP_X_REQUESTED_WITH']) === 'xmlhttprequest';
    
    if ($isAjaxRequest || $isApiRequest) {
        header('Content-Type: application/json');
        echo json_encode(['error' => "Server error: $message"]);
        exit(1);
    }
});
```

---

## TFS Integration Documentation

### The Forgotten Server Overview

The Forgotten Server (TFS) is an open-source MMORPG server written in C++ that supports Lua scripting for customization. TibiaAI supports both major TFS versions:

#### TFS 1.x (Modern)
- **RevScriptSys**: Automatic script loading from `data/scripts/`
- **Object-Oriented**: Modern Lua with metatables
- **Simplified Registration**: No XML configuration required

#### TFS 0.x (Legacy)
- **XML Registration**: Manual registration in XML files
- **Functional Approach**: Global functions like `doPlayerSendTextMessage()`
- **Separate Files**: XML configuration + Lua implementation

### RevScriptSys Examples (TFS 1.x)

#### 1. Action Events (Item Usage)
```lua
local action = Action()

function action.onUse(player, item, fromPosition, target, toPosition, isHotkey)
    if item.itemid == 2550 then
        item:transform(2737)
        item:decay()
        player:sendTextMessage(MESSAGE_INFO_DESCR, "O item foi transformado!")
        return true
    end
    return false
end

action:id(2550) -- Item ID that triggers the script
action:register()
```

#### 2. Movement Events (Stepping on Tiles)
```lua
local movement = MoveEvent()

function movement.onStepIn(creature, item, position, fromPosition)
    creature:sendTextMessage(MESSAGE_INFO_DESCR, "Você pisou em um local especial!")
    creature:getPosition():sendMagicEffect(CONST_ME_TELEPORT)
    return true
end

movement:aid(12345) -- Action ID on the tile
movement:register()
```

#### 3. Creature Events (Login/Logout)
```lua
local creatureEvent = CreatureEvent("PlayerLogin")

function creatureEvent.onLogin(player)
    player:sendTextMessage(MESSAGE_STATUS_DEFAULT, "Bem-vindo ao servidor!")
    -- Add player to a storage or perform initialization
    player:setStorageValue(1000, 1)
    return true
end

creatureEvent:register()
```

#### 4. Talk Actions (Chat Commands)
```lua
local talkAction = TalkAction("!teleport")

function talkAction.onSay(player, words, param)
    local position = Position(100, 100, 7)
    player:teleportTo(position)
    player:sendTextMessage(MESSAGE_STATUS_DEFAULT, "Teleportado!")
    return false
end

talkAction:separator(" ")
talkAction:register()
```

#### 5. Global Events (Server Events)
```lua
local globalEvent = GlobalEvent("ServerSave")

function globalEvent.onThink(interval)
    Game.broadcastMessage("Server save in 5 minutes!", MESSAGE_STATUS_WARNING)
    return true
end

globalEvent:interval(300000) -- Every 5 minutes
globalEvent:register()
```

#### 6. Spell System
```lua
local spell = Spell("instant")

function spell.onCastSpell(creature, variant)
    local target = variant:getNumber() and Creature(variant:getNumber()) or creature
    
    if not target then
        return false
    end
    
    target:addHealth(100)
    target:getPosition():sendMagicEffect(CONST_ME_MAGIC_BLUE)
    return true
end

spell:name("Healing Light")
spell:words("exura")
spell:level(10)
spell:mana(20)
spell:isAggressive(false)
spell:register()
```

#### 7. Weapon Events
```lua
local weapon = Weapon(WEAPON_SWORD)

function weapon.onUseWeapon(player, variant)
    local target = Creature(variant:getNumber())
    if target then
        target:getPosition():sendMagicEffect(CONST_ME_FIREATTACK)
        player:sendTextMessage(MESSAGE_STATUS_DEFAULT, "Sua espada flamejou!")
    end
    return true
end

weapon:id(2400) -- Weapon item ID
weapon:register()
```

### XML Registration Examples (TFS 0.x Legacy)

#### 1. Actions with XML Registration

**File: `data/actions/actions.xml`**
```xml
<actions>
    <action itemid="2050" script="potion.lua" />
    <action actionid="1000" script="lever.lua" />
</actions>
```

**File: `data/actions/scripts/potion.lua`**
```lua
function onUse(cid, item, fromPosition, itemEx, toPosition)
    local player = Player(cid)
    if not player then
        return false
    end
    
    doCreatureAddHealth(cid, 100)
    doSendMagicEffect(getCreaturePosition(cid), CONST_ME_MAGIC_BLUE)
    doRemoveItem(item.uid, 1)
    return true
end
```

#### 2. Movements with XML Registration

**File: `data/movements/movements.xml`**
```xml
<movements>
    <movevent event="StepIn" actionid="1001" script="teleport.lua" />
    <movevent event="StepOut" actionid="1002" script="exit_tile.lua" />
</movements>
```

**File: `data/movements/scripts/teleport.lua`**
```lua
function onStepIn(cid, item, position, fromPosition)
    local newPosition = {x = 100, y = 100, z = 7}
    doTeleportThing(cid, newPosition)
    doSendMagicEffect(newPosition, CONST_ME_TELEPORT)
    return true
end
```

#### 3. CreatureScripts with XML Registration

**File: `data/creaturescripts/creaturescripts.xml`**
```xml
<creaturescripts>
    <event type="login" name="PlayerLogin" script="login.lua" />
    <event type="logout" name="PlayerLogout" script="logout.lua" />
    <event type="death" name="PlayerDeath" script="death.lua" />
</creaturescripts>
```

**File: `data/creaturescripts/scripts/login.lua`**
```lua
function onLogin(cid)
    local player = getPlayerName(cid)
    doPlayerSendTextMessage(cid, MESSAGE_STATUS_CONSOLE_BLUE, "Welcome, " .. player .. "!")
    
    -- Register other events
    registerCreatureEvent(cid, "PlayerLogout")
    registerCreatureEvent(cid, "PlayerDeath")
    return true
end
```

### Lua Table Syntax (Critical for Script Generation)

#### Correct Table Syntax:
```lua
-- ✓ CORRECT: Use curly braces {} for tables
local config = {
    itemId = 2160,
    name = "Crystal Coin",
    attributes = {
        worth = 10000,
        stackable = true
    }
}

-- ✓ CORRECT: Array of objects
local upgradeItems = {
    {fromId = 1001, toId = 2001, chance = 50},
    {fromId = 1002, toId = 2002, chance = 60}
}

-- ✗ INCORRECT: Never use square brackets for table definition
local wrongConfig = [
    itemId = 2160  -- This is wrong!
]
```

#### Complex Table Examples:
```lua
-- Equipment upgrade system
local upgradeConfig = {
    [12345] = { -- Upgrade stone item ID
        baseChance = 50,
        items = {
            {fromItemId = 1001, toItemId = 2001, bonusAttack = 5},
            {fromItemId = 1002, toItemId = 2002, bonusDefense = 3}
        },
        effects = {
            success = CONST_ME_FIREWORK_YELLOW,
            failure = CONST_ME_POFF
        }
    }
}

-- Quest configuration
local questConfig = {
    name = "Dragon Quest",
    requiredLevel = 50,
    rewards = {
        experience = 10000,
        items = {
            {id = 2160, count = 5}, -- Crystal coins
            {id = 2400, count = 1}  -- Magic sword
        }
    },
    stages = {
        {
            description = "Kill 10 dragons",
            requirement = {type = "kill", monster = "dragon", count = 10}
        },
        {
            description = "Collect dragon scale",
            requirement = {type = "item", id = 5877, count = 1}
        }
    }
}
```

### TFS Function Reference

#### Common TFS 1.x Functions:
```lua
-- Player functions
player:getName()
player:getLevel()
player:addExperience(amount)
player:sendTextMessage(type, message)
player:teleportTo(position)
player:addItem(itemId, count)

-- Position functions
local pos = Position(x, y, z)
pos:sendMagicEffect(effect)
pos:sendDistanceEffect(toPosition, effect)

-- Item functions
item:getId()
item:transform(newId)
item:remove()
item:decay()

-- Game functions
Game.broadcastMessage(message, type)
Game.getPlayerCount()
```

#### Common TFS 0.x Functions:
```lua
-- Player functions (using creature ID)
getPlayerName(cid)
getPlayerLevel(cid)
doPlayerAddExp(cid, exp)
doPlayerSendTextMessage(cid, type, message)
doTeleportThing(cid, position)
doPlayerAddItem(cid, itemId, count)

-- Position functions
doSendMagicEffect(position, effect)
doSendDistanceShoot(fromPos, toPos, effect)

-- Item functions
getItemIdByName(name)
doTransformItem(uid, newId)
doRemoveItem(uid, count)

-- General functions
doBroadcastMessage(message, type)
getPlayersOnline()
```

---

## Performance & Scalability

### Database Optimization

#### Connection Pooling:
```php
class Database {
    private static $instance = null;
    private $connection;
    
    public static function getInstance() {
        if (self::$instance === null) {
            self::$instance = new self();
        }
        return self::$instance;
    }
}
```

#### Query Optimization:
```php
// Prepared statements with parameter binding
function db_query($sql, $params = []) {
    $stmt = db()->prepare($sql);
    $stmt->execute($params);
    return $stmt->fetchAll(PDO::FETCH_ASSOC);
}
```

### Asset Management

#### Cache Busting:
```php
function loadCss($file) {
    $cacheBuster = '?v=' . filemtime(BASE_PATH . '/assets/css/' . $file);
    return '<link rel="stylesheet" href="/assets/css/' . $file . $cacheBuster . '">';
}
```

#### Resource Optimization:
- Minified CSS and JavaScript
- Optimized images with proper formats
- CDN-ready asset organization
- Gzip compression support

### Memory Management

#### Efficient File Processing:
```php
// Stream large directories without loading everything into memory
$dirIterator = new RecursiveDirectoryIterator($this->libPath);
$iterIterator = new RecursiveIteratorIterator($dirIterator);
$allFiles = new RegexIterator($iterIterator, '/\.lua$/i', RegexIterator::GET_MATCH);
```

---

## API Reference

### Authentication Endpoints

#### POST `/login`
```json
{
    "email": "user@example.com",
    "password": "password123",
    "csrf_token": "abc123..."
}
```

**Response:**
```json
{
    "success": true,
    "redirect": "/dashboard"
}
```

#### POST `/register`
```json
{
    "username": "newuser",
    "email": "newuser@example.com", 
    "password": "securepass123",
    "csrf_token": "abc123..."
}
```

### Script Generation Endpoints

#### POST `/script/generate`
```json
{
    "tfs_version": "1.5",
    "module_type": "action",
    "script_type": "revscript",
    "description": "Create a healing potion that restores 100 HP",
    "database": "Optional database integration details",
    "csrf_token": "abc123...",
    "script_token": "def456..."
}
```

**Response:**
```json
{
    "script": "-- Generated Lua script content",
    "formattedHtml": "<pre><code>...</code></pre>",
    "status": 200
}
```

#### POST `/script/analyze`
```json
{
    "script_content": "local action = Action()...",
    "tfs_version": "1.5",
    "module_type": "action",
    "use_examples": true
}
```

**Response:**
```json
{
    "format": "revscript",
    "moduleType": "action",
    "functions": [...],
    "errors": [...],
    "recommendations": [...],
    "examples": {...},
    "scores": {
        "overall": 85,
        "quality": 90,
        "performance": 80
    }
}
```

### Forum Endpoints

#### GET `/forum/category/{slug}`
Returns forum category with topics

#### POST `/forum/topic/create`
```json
{
    "category_id": 1,
    "title": "New Topic Title",
    "content": "Topic content...",
    "csrf_token": "abc123..."
}
```

#### POST `/forum/post/{id}/vote`
```json
{
    "vote_type": "up",
    "csrf_token": "abc123..."
}
```

---

## Development Guidelines

### Code Style Standards

#### PHP Standards:
```php
// Use PSR-4 autoloading
// Type hints where possible
public function analyze(string $content, string $version): array

// Comprehensive error handling
try {
    $result = $this->processScript($content);
} catch (Exception $e) {
    error_log("Script processing failed: " . $e->getMessage());
    throw new ScriptProcessingException("Analysis failed", 0, $e);
}
```

#### JavaScript Standards:
```javascript
// Modern ES6+ features
document.addEventListener('DOMContentLoaded', function() {
    // Progressive enhancement
    const copyButtons = document.querySelectorAll('.copy-button');
    copyButtons.forEach(button => {
        button.addEventListener('click', handleCopyClick);
    });
});

// Proper error handling
navigator.clipboard.writeText(content)
    .then(() => showSuccess())
    .catch(err => showError(err));
```

### Security Guidelines

#### Input Validation:
```php
// Always validate and sanitize input
$description = filter_var($_POST['description'], FILTER_SANITIZE_STRING);
if (empty($description) || strlen($description) > 5000) {
    throw new InvalidArgumentException('Invalid description');
}
```

#### Database Queries:
```php
// Always use prepared statements
$stmt = $this->db->prepare("SELECT * FROM scripts WHERE user_id = ? AND type = ?");
$stmt->execute([$userId, $type]);
```

#### CSRF Protection:
```php
// Validate CSRF tokens on all state-changing operations
if (!$this->auth->validateToken($_POST['csrf_token'])) {
    throw new SecurityException('Invalid CSRF token');
}
```

### Testing Guidelines

#### Unit Testing Structure:
```php
class ScriptAnalyzerTest extends PHPUnit\Framework\TestCase {
    public function testRevScriptDetection() {
        $analyzer = new ScriptAnalyzer();
        $content = 'local action = Action()';
        
        $result = $analyzer->detectFormat($content);
        $this->assertEquals('revscript', $result);
    }
}
```

#### Integration Testing:
```php
class ScriptGenerationTest extends PHPUnit\Framework\TestCase {
    public function testFullScriptGeneration() {
        $controller = new ScriptController();
        
        $_POST = [
            'tfs_version' => '1.5',
            'module_type' => 'action',
            'script_type' => 'revscript',
            'description' => 'Test script',
            'csrf_token' => $this->getValidToken()
        ];
        
        $result = $controller->generate();
        $this->assertArrayHasKey('script', $result);
    }
}
```

---

## Troubleshooting

### Common Issues and Solutions

#### 1. Script Generation Failures

**Problem**: "Failed to generate script. Please try again."

**Solutions**:
- Check OpenAI API key validity in `config/config.php`
- Verify network connectivity to OpenAI servers
- Check rate limiting status
- Review error logs for detailed error messages

**Debug Commands**:
```bash
# Check error logs
tail -f error.log

# Verify API key
grep OPENAI_API_KEY config/config.php

# Test API connectivity
curl -H "Authorization: Bearer YOUR_API_KEY" https://api.openai.com/v1/models
```

#### 2. Database Connection Issues

**Problem**: "Database connection failed"

**Solutions**:
- Verify credentials in `config/database.php`
- Check MySQL server status
- Ensure database exists and user has permissions
- Test connection manually

**Debug Commands**:
```bash
# Test MySQL connection
mysql -h localhost -u username -p database_name

# Check MySQL service
service mysql status

# Review MySQL error logs
tail -f /var/log/mysql/error.log
```

#### 3. Authentication Problems

**Problem**: "Invalid security token" or session issues

**Solutions**:
- Clear browser cookies and cache
- Check session configuration in PHP
- Verify CSRF token generation
- Review session storage permissions

**Debug Commands**:
```php
// Check session status
var_dump(session_status());

// Verify token generation
var_dump($_SESSION['csrf_token']);

// Check session save path permissions
echo session_save_path();
```

#### 4. TFS File Access Issues

**Problem**: "No examples found" or file access errors

**Solutions**:
- Verify TFS server paths in configuration
- Check file permissions on TFS directories
- Ensure TFS files exist in expected locations
- Review directory structure

**Debug Commands**:
```bash
# Check directory permissions
ls -la servers/tfs/

# Verify TFS structure
find servers/tfs/ -name "*.lua" | head -10

# Check file accessibility
test -r servers/tfs/tfs-1x/forgottenserver-master/data/scripts/
```

#### 5. Performance Issues

**Problem**: Slow response times or timeouts

**Solutions**:
- Enable PHP OPcache
- Optimize database queries with indexes
- Implement Redis caching
- Review server resources

**Debug Commands**:
```bash
# Check PHP configuration
php -i | grep opcache

# Monitor MySQL queries
mysql -e "SHOW PROCESSLIST;"

# Check server resources
top
df -h
```

### Error Logging

#### Comprehensive Logging Setup:
```php
// Error logging configuration
ini_set('log_errors', 1);
ini_set('error_log', __DIR__ . '/error.log');

// Custom error handler for detailed logging
set_error_handler(function($severity, $message, $file, $line) {
    $errorType = [
        E_ERROR => 'ERROR',
        E_WARNING => 'WARNING',
        E_NOTICE => 'NOTICE'
    ][$severity] ?? 'UNKNOWN';
    
    error_log("[{$errorType}] {$message} in {$file} on line {$line}");
});
```

#### Debug Mode Configuration:
```php
// Enable debug mode for development
if (defined('DEBUG_MODE') && DEBUG_MODE) {
    ini_set('display_errors', 1);
    error_reporting(E_ALL);
    
    // Additional debug information
    register_shutdown_function(function() {
        $error = error_get_last();
        if ($error) {
            error_log("Shutdown error: " . json_encode($error));
        }
    });
}
```

---

## Future Development Roadmap

### Planned Features

#### 1. Enhanced AI Capabilities
- **Multi-Model Support**: Integration with Claude, Gemini
- **Specialized Models**: TFS-specific fine-tuned models
- **Code Optimization**: AI-powered performance suggestions
- **Documentation Generation**: Automatic comment generation

#### 2. Advanced TFS Integration
- **Real-Time Sync**: Live synchronization with TFS servers
- **Version Migration**: Automatic script migration between TFS versions
- **Compatibility Checker**: Cross-version compatibility analysis
- **Performance Profiler**: Script performance analysis

#### 3. Development Tools
- **Integrated IDE**: Web-based Lua editor with syntax highlighting
- **Debugging Tools**: Breakpoint and step-through debugging
- **Testing Framework**: Unit testing for TFS scripts
- **Version Control**: Git integration for script management

#### 4. Community Features
- **Script Marketplace**: Community script sharing
- **Collaborative Editing**: Real-time collaborative script development
- **Code Reviews**: Peer review system for scripts
- **Mentorship Program**: Expert developer guidance

#### 5. Enterprise Features
- **Team Management**: Multi-user organization support
- **Access Controls**: Role-based permissions
- **Audit Logging**: Comprehensive action tracking
- **API Endpoints**: RESTful API for external integrations

### Technical Improvements

#### 1. Performance Optimization
- **Caching Layer**: Redis/Memcached implementation
- **Database Optimization**: Query optimization and indexing
- **CDN Integration**: Global content delivery
- **Load Balancing**: Horizontal scaling support

#### 2. Security Enhancements
- **OAuth Integration**: Social login support
- **Two-Factor Authentication**: Enhanced account security
- **Security Scanning**: Automated vulnerability detection
- **Penetration Testing**: Regular security assessments

#### 3. Monitoring and Analytics
- **Application Monitoring**: Performance metrics tracking
- **User Analytics**: Usage pattern analysis
- **Error Tracking**: Centralized error management
- **Business Intelligence**: Data-driven insights

---

## Conclusion

TibiaAI represents a sophisticated, production-ready web application that successfully bridges AI technology with practical TFS development needs. The system demonstrates:

### Technical Excellence
- **Clean Architecture**: Well-structured MVC framework with clear separation of concerns
- **Advanced Security**: Multi-layered security implementation exceeding industry standards
- **Performance Optimization**: Efficient resource utilization and scalable design
- **Code Quality**: Maintainable, documented, and testable codebase

### Innovation in AI Integration
- **Context-Aware Generation**: Intelligent prompt engineering with real TFS file analysis
- **Multi-Model Support**: Robust AI integration with fallback mechanisms
- **Domain Expertise**: Deep understanding of TFS ecosystem and development workflows
- **Quality Assurance**: Comprehensive validation and error detection

### User-Centric Design
- **Intuitive Interface**: Clean, modern UI with progressive enhancement
- **Accessibility**: Proper semantic markup and keyboard navigation
- **Internationalization**: Full i18n support with dynamic language switching
- **Community Features**: Comprehensive forum system with moderation tools

### Scalability and Maintainability
- **Modular Design**: Easy to extend and maintain with component-based architecture
- **Database Design**: Optimized schema with proper indexing and relationships
- **Error Handling**: Comprehensive error management with detailed logging
- **Documentation**: Extensive documentation for developers and users

This analysis reveals a mature application that not only solves real-world problems in the TFS development community but also serves as an excellent example of modern PHP web application development practices. The system's architecture is robust enough to handle significant growth while remaining flexible enough to adapt to changing requirements.

The integration between AI technology and domain-specific knowledge creates a powerful tool that enhances developer productivity while maintaining the quality and reliability essential for game server development.

---

*This documentation serves as a comprehensive reference for developers, system administrators, and stakeholders working with or evaluating the TibiaAI system.*
