# MRrobot-MEWIZARD
 NOT SURE  JUST  HACK THE PLANET!!!!!!!
<#
.SYNOPSIS
    MR. ROBOT ULTIMATE – FINAL BULLETPROOF VERSION
    Privacy + Pentest + OSINT + Printer Toolkit
.NOTES
    Run as Administrator. All issues fixed.
#>

#requires -RunAsAdministrator

# ========== Auto-elevate ==========
if (-NOT ([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole] "Administrator")) {
    Write-Host "Requesting administrator privileges..." -ForegroundColor Yellow
    Start-Process powershell.exe -ArgumentList "-NoProfile -ExecutionPolicy Bypass -File `"$PSCommandPath`"" -Verb RunAs
    exit
}
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force

# ========== Configuration ==========
$OUTPUT_DIR = "$env:USERPROFILE\Desktop\MR_ROBOT_$(Get-Date -Format 'yyyyMMdd_HHmmss')"
$MODULES_DIR = "$OUTPUT_DIR\modules"
$LOGS_DIR = "$OUTPUT_DIR\logs"
$SCANS_DIR = "$OUTPUT_DIR\scans"
$TOOLS_DIR = "$OUTPUT_DIR\github_tools"
$BTC_DIR = "$OUTPUT_DIR\bitcoin_wallet"
$RANDOM_DIR = "$OUTPUT_DIR\random_content"
$PRINTER_LOG = "$OUTPUT_DIR\printer_log.txt"
$OSINT_DIR = "$OUTPUT_DIR\osint_reports"
$TOR_DATA_DIR = "C:\ProgramData\tor"
New-Item -ItemType Directory -Force -Path $MODULES_DIR, $LOGS_DIR, $SCANS_DIR, $TOOLS_DIR, $BTC_DIR, $RANDOM_DIR, $OSINT_DIR | Out-Null

$LOG = "$LOGS_DIR\master.log"
$DEVICES_CSV = "$OUTPUT_DIR\devices.csv"
$SUMMARY_TXT = "$OUTPUT_DIR\summary.txt"

# ========== Contact / OSINT Info ==========
$CONTACT_EMAIL = "ranfom@gmail.com"
$CONTACT_PHONE = "(718) 422-0200"
$CONTACT_ADDRESS = "22 W 88th St Apt 1, New York, NY 10024-2549"
$CONTACT_SINCE = "Since 2018"

# OSINT Target 1: Joseph McMoneagle
$OSINT_JOSEPH = @"
JOSEPH WILLIAM MCMONEAGLE
Born: 1946 | Age: 80
Address: 185 Coleman Pl, Nellysford, VA 22958-9504
Email: jmceagle@cstone.net
Phone: (434) 361-1516
Spouse: Nancy Mcmoneagle
Aliases: Joe W Mcmoneagle, Joseph N Mcmoneagle
Education: Excelsior College
Previous Addresses:
- Po Box 100, Nellysford, VA 22958-0100 (2019-2025)
- 1530 Roberts Mountain Rd, Faber, VA 22938-2324 (1984-2023)
- 62 Roberts Mountain Rd, Faber, VA 22938-2317 (1996-2020)
- Po Box 642, Fort George G Meade, MD 20755-0642 (2004)
- 100 Rr 1, Nellysford, VA (1984-1991)
Data Breaches: jmceagle@cstone.net (8 incidents)
"@

# OSINT Target 2: Vera Farmiga
$OSINT_VERA = @"
VERA A FARMIGA
Born: 1974 | Age: 52
Address: 22 W 88th St Apt 1, New York, NY 10024-2549
Email: ranfom@gmail.com
Phone: (718) 422-0200
Spouse: Sebastian Roche
Aliases: Vera A Roche, Vera Sarmiga
Parents: Luba Farmiga (77), Michael Farmiga (83)
Siblings: Alexander (37), Stephan (50), Laryssa (35), Nadia Costa (49), Victor (54)
Previous Addresses:
- 672 Mettacahonts Rd, Accord, NY 12404-5925 (2025-2026)
- 327 Cpw, New York, NY 10025 (2004-2026)
- Po Box 418, West Park, NY 12493-0418 (2020)
- 14 Moon Shadow Ln, Asheville, NC 28805-0002 (2000-2017)
- 200 Valli Rd, Highland, NY 12528 (2016)
- 5 Mountain View Rd, Whitehouse Station, NJ 08889-3362 (2001-2005)
Data Breaches: ranfom@gmail.com (17 incidents)
"@

# ========== ASCII Art ==========
$JOKER_ART = @"
        ██╗ ██████╗ ██╗  ██╗███████╗██████╗
        ██║██╔═══██╗██║ ██╔╝██╔════╝██╔══██╗
        ██║██║   ██║█████╔╝ █████╗  ██████╔╝
   ██   ██║██║   ██║██╔═██╗ ██╔══╝  ██╔══██╗
   ╚█████╔╝╚██████╔╝██║  ██╗███████╗██║  ██║
    ╚════╝  ╚═════╝ ╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝
"@

$MEWTWO_ART = @"
        ███╗   ███╗███████╗██╗    ██╗████████╗██╗    ██╗ ██████╗
        ████╗ ████║██╔════╝██║    ██║╚══██╔══╝██║    ██║██╔═══██╗
        ██╔████╔██║█████╗  ██║ █╗ ██║   ██║   ██║ █╗ ██║██║   ██║
        ██║╚██╔╝██║██╔══╝  ██║███╗██║   ██║   ██║███╗██║██║   ██║
        ██║ ╚═╝ ██║███████╗╚███╔███╔╝   ██║   ╚███╔███╔╝╚██████╔╝
        ╚═╝     ╚═╝╚══════╝ ╚══╝╚══╝    ╚═╝    ╚══╝╚══╝  ╚═════╝
"@

$CHARIZARD_ART = @"
         ██████╗██╗  ██╗ █████╗ ██████╗ ██╗███████╗ █████╗ ██████╗ ██████╗
        ██╔════╝██║  ██║██╔══██╗██╔══██╗██║╚══███╔╝██╔══██╗██╔══██╗██╔══██╗
        ██║     ███████║███████║██████╔╝██║  ███╔╝ ███████║██████╔╝██████╔╝
        ██║     ██╔══██║██╔══██║██╔══██╗██║ ███╔╝  ██╔══██║██╔══██╗██╔══██╗
        ╚██████╗██║  ██║██║  ██║██║  ██║██║███████╗██║  ██║██║  ██║██║  ██║
         ╚═════╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝╚══════╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝
"@

# ========== Helper Functions ==========
function Write-Log {
    param([string]$Message, [string]$Color = "White")
    $timestamp = Get-Date -Format "HH:mm:ss"
    $logEntry = "[$timestamp] $Message"
    Add-Content -Path $LOG -Value $logEntry
    Write-Host $logEntry -ForegroundColor $Color
}

function Test-Command($cmd) {
    return [bool](Get-Command $cmd -ErrorAction SilentlyContinue)
}

# ========== 1. Random Content Generation ==========
function New-RandomContent {
    param([int]$FileCount = 10)
    Write-Log "Generating $FileCount random data files..." -Color Cyan
    1..$FileCount | ForEach-Object {
        $fileName = "$RANDOM_DIR\random_$(Get-Random -Maximum 9999).txt"
        $content = @"
Random File: $_
Generated: $(Get-Date)
Random Number: $(Get-Random -Minimum 1 -Maximum 1000000)
Random String: $( -join ((65..90) + (97..122) | Get-Random -Count 20 | ForEach-Object {[char]$_}) )
"@
        $content | Out-File $fileName
    }
    Write-Log "Generated $FileCount random files in $RANDOM_DIR" -Color Green
}

# ========== 2. Bitcoin Wallet ==========
function Generate-BitcoinWallet {
    Write-Log "Generating Bitcoin wallet..." -Color Cyan
    $walletFile = "$BTC_DIR\wallet_info.json"
    $passFile = "$BTC_DIR\wallet_password.txt"
    $seedFile = "$BTC_DIR\seed_phrase.txt"
    
    # Simulated wallet
    $privateKey = -join ((48..57) + (65..90) + (97..122) | Get-Random -Count 64 | ForEach-Object { [char]$_ })
    $publicKey = -join ((48..57) + (65..90) + (97..122) | Get-Random -Count 128 | ForEach-Object { [char]$_ })
    $address = "1" + -join ((48..57) + (65..90) + (97..122) | Get-Random -Count 33 | ForEach-Object { [char]$_ })
    $seed = -join ((48..57) + (65..90) + (97..122) | Get-Random -Count 12 | ForEach-Object { [char]$_ })
    $info = @{
        address = $address
        public_key = $publicKey
        private_key_wif = $privateKey
        network = "bitcoin"
    }
    $info | ConvertTo-Json | Out-File $walletFile
    "password_$(Get-Random -Maximum 999999)" | Out-File $passFile
    $seed | Out-File $seedFile
    Write-Log "Wallet files saved to $BTC_DIR" -Color Green
}

# ========== 3. Chocolatey Install ==========
function Install-ChocolateyIfMissing {
    if (-not (Test-Command "choco")) {
        Write-Log "Installing Chocolatey..." -Color Yellow
        Set-ExecutionPolicy Bypass -Scope Process -Force
        [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
        Invoke-Expression ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
        
        # Wait for Chocolatey to install and refresh PATH
        Write-Log "Waiting for Chocolatey to complete..." -Color Yellow
        Start-Sleep -Seconds 10
        refreshenv
    }
}

# ========== 4. Git Installation with PATH Fix ==========
function Install-GitWithPathFix {
    Write-Log "Installing Git with PATH fix..." -Color Cyan
    
    Install-ChocolateyIfMissing
    
    # Install Git
    choco install git -y --no-progress
    
    Write-Log "Git installed. Refreshing environment..." -Color Yellow
    Start-Sleep -Seconds 5
    
    # Manually add Git to PATH
    $gitPaths = @(
        "C:\Program Files\Git\bin",
        "C:\Program Files\Git\cmd",
        "C:\ProgramData\chocolatey\lib\git\tools\git\bin",
        "C:\ProgramData\chocolatey\lib\git\tools\git\cmd"
    )
    
    $currentPath = [Environment]::GetEnvironmentVariable("Path", "Machine")
    foreach ($path in $gitPaths) {
        if (Test-Path $path -and $currentPath -notlike "*$path*") {
            Write-Log "Adding $path to PATH..." -Color Yellow
            $currentPath = "$currentPath;$path"
        }
    }
    
    [Environment]::SetEnvironmentVariable("Path", $currentPath, "Machine")
    
    # Also update current session PATH
    $env:Path = [Environment]::GetEnvironmentVariable("Path", "Machine")
    
    # Verify Git
    if (Get-Command git -ErrorAction SilentlyContinue) {
        Write-Log "Git successfully installed and available." -Color Green
        return $true
    } else {
        Write-Log "Git installation may require a reboot. Will continue without cloning." -Color Red
        return $false
    }
}

# ========== 5. Tor Service Installation ==========
function Install-TorService {
    Write-Log "Setting up Tor as Windows service..." -Color Cyan
    
    Install-ChocolateyIfMissing
    
    # Install Tor Expert Bundle
    choco install tor -y --force --no-progress
    
    # Wait for installation
    Start-Sleep -Seconds 5
    
    # Find tor.exe
    $possiblePaths = @(
        "C:\ProgramData\chocolatey\lib\tor\tools\tor\tor.exe",
        "C:\ProgramData\chocolatey\lib\tor\tools\Tor\tor.exe",
        "C:\Program Files\Tor\tor.exe"
    )
    
    $TorExe = $null
    foreach ($path in $possiblePaths) {
        if (Test-Path $path) {
            $TorExe = $path
            break
        }
    }
    
    if (-not $TorExe) {
        Write-Log "Tor executable not found. Tor Browser may be required." -Color Yellow
        return
    }
    
    # Create directories
    New-Item -Force -ItemType Directory "$TOR_DATA_DIR\data" | Out-Null
    
    # Create torrc
    $torrc = "$TOR_DATA_DIR\torrc"
    $torrcContent = @"
DataDirectory $TOR_DATA_DIR\data
ControlPort 9051
CookieAuthentication 1
SOCKSPort 127.0.0.1:9050
ExitNodes {us},{de},{nl},{fr},{gb},{ca}
StrictNodes 1
"@
    Set-Content -Encoding ASCII $torrc -Value $torrcContent
    
    # Set permissions
    icacls $TOR_DATA_DIR /grant "NT AUTHORITY\LOCAL SERVICE:(OI)(CI)F" /T 2>$null
    
    # Try to install service
    try {
        & $TorExe --service install --options -f $torrc 2>$null
    } catch {
        Write-Log "Tor service may already be installed." -Color Yellow
    }
    
    # Start service
    sc.exe config tor start= auto 2>$null
    Start-Service tor -ErrorAction SilentlyContinue
    
    Write-Log "Tor service configured on port 9050 (SOCKS5)" -Color Green
}

# ========== 6. GitHub Repository Cloner (FIXED with PATH handling) ==========
function Clone-AllRepos {
    Write-Log "Starting to clone GitHub repositories..." -Color Cyan
    
    # Install and configure Git
    $gitAvailable = Install-GitWithPathFix
    
    if (-not $gitAvailable) {
        Write-Log "Git not available. Cannot clone repositories." -Color Red
        return
    }
    
    # Refresh PATH in current session
    $env:Path = [Environment]::GetEnvironmentVariable("Path", "Machine")
    
    # Configure Git to bypass SSL issues
    git config --global http.sslVerify false
    
    $repos = @(
        "https://github.com/karma9874/AndroRAT.git",
        "https://github.com/shivaya-dav/DogeRat.git",
        "https://github.com/nathanlopez/Stitch.git",
        "https://github.com/stamparm/EternalRocks.git",
        "https://github.com/NYAN-x-CAT/Lime-RAT.git",
        "https://github.com/AhmetHan/h-worm_houdini.git",
        "https://github.com/SaladinoBelisario/RAT_Rust.git",
        "https://github.com/hktkqwe123/All-Hacking-Tools.git",
        "https://github.com/ebadfd/hack-gmail.git",
        "https://github.com/Ha3MrX/Gemail-Hack.git",
        "https://github.com/roarrraor/whathehack.git",
        "https://github.com/Zisanhacker99/DarkWebhacker99.git",
        "https://github.com/kpcyrd/sniffglue.git",
        "https://github.com/buckyroberts/Python-Packet-Sniffer.git",
        "https://github.com/EONRaider/Packet-Sniffer.git",
        "https://github.com/muscledreamer/twitter_scrapy.git",
        "https://github.com/rick-hup/amazon-crwaler.git",
        "https://github.com/pentas1150/google-scholar-keyword-crwaler.git",
        "https://github.com/byt3bl33d3r/MITMf.git",
        "https://github.com/giriaryan694-a11y/Awsome-Search-Engines-For-CyberSec.git",
        "https://github.com/Moham3dRiahi/XAttacker.git",
        "https://github.com/btcsuite/btcwallet.git",
        "https://github.com/gurnec/btcrecover.git",
        "https://github.com/Cvar1984/pemulungBTC.git",
        "https://github.com/ThinkEnigmatic/polymarket-bot-arena.git",
        "https://github.com/chrishah/MITObim.git",
        "https://github.com/the-cult-of-integral/Scambaiting-Setup.git",
        "https://github.com/PreTeXtBook/pretext.git",
        "https://github.com/L4bF0x/PhishingPretexts.git",
        "https://github.com/r00t-3xp10it/Samsung-TV-Denial-of-Service-DoS-Attack.git",
        "https://github.com/flashnuke/deadnet.git",
        "https://github.com/maxkrivich/SlowLoris.git",
        "https://github.com/MeherRushi/FlowSentryX.git",
        "https://github.com/jkramarz/TheTick.git",
        "https://github.com/hc-nolan/aitm-demo.git",
        "https://github.com/vinzabe/AITM-Attack-Detector.git",
        "https://github.com/pankajmore/ip_spoofing.git",
        "https://github.com/sr2echa/Monotone-HWID-Spoofer.git",
        "https://github.com/ParsaKSH/spoof-tunnel.git",
        "https://github.com/uklans/cache-domains.git",
        "https://github.com/DanMcInerney/dnsspoof.git",
        "https://github.com/RedTeamPentesting/pretender.git",
        "https://github.com/sharkdp/bat.git",
        "https://github.com/KevinWang676/Bark-Voice-Cloning.git",
        "https://github.com/pvorb/clone.git",
        "https://github.com/GorvGoyl/Clone-Wars.git",
        "https://github.com/myclabs/DeepCopy.git",
        "https://github.com/xming521/WeClone.git",
        "https://github.com/sqlmapproject/sqlmap.git",
        "https://github.com/kleiton0x00/Advanced-SQL-Injection-Cheatsheet.git",
        "https://github.com/the-robot/sqliv.git",
        "https://github.com/sensepost/jack.git",
        "https://github.com/shifa123/clickjackingpoc.git",
        "https://github.com/dxa4481/XSSJacking.git",
        "https://github.com/D4Vinci/Clickjacking-Tester.git",
        "https://github.com/AgainstTheWest/NginxDay.git",
        "https://github.com/Anandesh-Sharma/0Day-Exploits.git",
        "https://github.com/IAmBlackHacker/Facebook-BruteForce.git",
        "https://github.com/MorDavid/BruteForceAI.git",
        "https://github.com/Antu7/python-bruteForce.git",
        "https://github.com/jwilger/jack-the-ripper.git",
        "https://github.com/vanhauser-thc/thc-hydra.git",
        "https://github.com/hashcat/hashcat.git",
        "https://github.com/medusajs/medusa.git",
        "https://github.com/cyclops-ui/cyclops.git",
        "https://github.com/blackears/cyclopsLevelBuilder.git",
        "https://github.com/deadpool-rs/deadpool.git",
        "https://github.com/SideChannelMarvels/Deadpool.git",
        "https://github.com/thinkoaa/Deadpool.git",
        "https://github.com/byt3bl33d3r/SprayingToolkit.git",
        "https://github.com/d-oliveros/nightcrawler.git",
        "https://github.com/tulios/nightcrawler_swift.git",
        "https://github.com/Shopify/wolverine.git",
        "https://github.com/progrium/wolverine.git",
        "https://github.com/THUYimingLi/BackdoorBox.git",
        "https://github.com/bboylyg/BackdoorLLM.git",
        "https://github.com/xmrig/xmrig.git",
        "https://github.com/xmrig/xmrig-proxy.git",
        "https://github.com/xmrig/xmrig-nvidia.git",
        "https://github.com/xmrig/xmrig-amd.git",
        "https://github.com/xmrig/xmrig-cuda.git",
        "https://github.com/xmrig/xmrig-deps.git",
        "https://github.com/gupax-io/gupax.git",
        "https://github.com/tyler-young/Mind-Controlled-Wheelchair.git",
        "https://github.com/bolt-foundry/gambit.git",
        "https://github.com/gambitproject/gambit.git",
        "https://github.com/stanford-oval/storm.git",
        "https://github.com/nathanmarz/storm.git",
        "https://github.com/machineagency/jubilee.git",
        "https://github.com/isaiah/jubilee.git",
        "https://github.com/SteveJWallace/JubileeKlipper.git",
        "https://github.com/hackthebox/cyber-apocalypse-2024.git",
        "https://github.com/amazon-archives/aws-lambda-zombie-workshop.git",
        "https://github.com/Arcainex/The-Apocalypse-Project.git",
        "https://github.com/hackthebox/cyber-apocalypse-2025.git",
        "https://github.com/SparksScripts/Total-Apocalypse.git",
        "https://github.com/OpenApoc/OpenApoc.git",
        "https://github.com/USBBios/Joker-Mirai-Botnet-Source-V1.git",
        "https://github.com/iconic05/Queen-Ruva-ai-Beta.git",
        "https://github.com/charlesbel/Shopify-Checkout-Bypasser.git",
        "https://github.com/frddl/scribd-bypasser.git",
        "https://github.com/xaitax/Chrome-App-Bound-Encryption-Decryption.git",
        "https://github.com/MarcoPolo/Mac-n-Cheese.git",
        "https://github.com/akiba-hs/cups.git",
        "https://github.com/apple/cups.git",
        "https://github.com/OpenPrinting/cups.git",
        "https://github.com/harwey/cups4j.git",
        "https://github.com/quadportnick/docker-cups-airprint.git",
        "https://github.com/OpenPrinting/system-config-printer.git"
    ) | Sort-Object -Unique

    Write-Log "Found $($repos.Count) unique repositories." -Color Green
    
    Write-Host "`n" -ForegroundColor Red
    Write-Host "  LEGAL WARNING" -ForegroundColor Red
    Write-Host "These tools include RATs, bypassers, and hacking tools that may be illegal to use without authorization." -ForegroundColor Yellow
    Write-Host "By proceeding, you confirm you are downloading these for educational/research purposes only." -ForegroundColor Yellow
    $confirm = Read-Host "`nDo you want to continue downloading these tools? (y/N)"
    if ($confirm -ne 'y') {
        Write-Log "Tool download cancelled." -Color Yellow
        return
    }

    Push-Location $TOOLS_DIR
    $successCount = 0
    foreach ($repo in $repos) {
        $repoName = $repo -replace '.*/(.*)\.git', '$1'
        Write-Host "Cloning $repoName ... " -NoNewline
        
        try {
            $env:GIT_SSL_NO_VERIFY = "1"
            $result = git clone $repo 2>&1
            if ($LASTEXITCODE -eq 0 -and (Test-Path $repoName)) {
                Write-Host "OK" -ForegroundColor Green
                $successCount++
            } else {
                Write-Host "FAILED" -ForegroundColor Red
            }
        } catch {
            Write-Host "FAILED" -ForegroundColor Red
        }
    }
    Pop-Location
    
    # Reset Git config
    git config --global http.sslVerify true
    
    Write-Log "Cloned $successCount of $($repos.Count) repositories to $TOOLS_DIR" -Color Green
}

# ========== 7. OSINT Report Generation ==========
function Save-OSINTReports {
    $OSINT_JOSEPH | Out-File "$OSINT_DIR\joseph_mcmoneagle.txt"
    $OSINT_VERA | Out-File "$OSINT_DIR\vera_farmiga.txt"
    Write-Log "OSINT reports saved to $OSINT_DIR" -Color Green
}

# ========== 8. Network Discovery ==========
function Invoke-NetworkDiscovery {
    Write-Log "Discovering local network..." -Color Cyan
    $localIP = (Get-NetIPAddress -AddressFamily IPv4 | Where-Object { $_.InterfaceAlias -notlike '*Loopback*' -and $_.PrefixOrigin -ne 'WellKnown' }).IPAddress | Select-Object -First 1
    if (-not $localIP) {
        Write-Log "Could not determine local IP address." -Color Red
        return @()
    }
    $network = ($localIP -split '\.')[0..2] -join '.'
    Write-Log "Local IP: $localIP, scanning subnet: $network.0/24" -Color Green

    $liveIPs = @()
    for ($i = 1; $i -le 254; $i++) {
        $ip = "$network.$i"
        if (Test-Connection -ComputerName $ip -Count 1 -Quiet -ErrorAction SilentlyContinue) {
            $liveIPs += $ip
            Write-Host "Found: $ip" -ForegroundColor Green
        }
    }
    Write-Log "Found $($liveIPs.Count) live hosts." -Color Green

    $deviceInfo = @()
    foreach ($ip in $liveIPs) {
        $arp = Get-NetNeighbor -IPAddress $ip -ErrorAction SilentlyContinue
        $mac = if ($arp) { $arp.LinkLayerAddress } else { "Unknown" }
        $deviceInfo += [PSCustomObject]@{ IP = $ip; MAC = $mac }
    }
    $deviceInfo | Export-Csv $DEVICES_CSV -NoTypeInformation
    $deviceInfo | Format-Table -AutoSize | Out-String | Write-Log
    return $deviceInfo
}

# ========== 9. Printer Testing Module ==========
function Test-NetworkPrinters {
    Write-Log "Starting printer testing module..." -Color Cyan
    
    # YOUR PRINTER IPs – edit these!
    $printerIPs = @(
        "192.168.1.100",  # Replace with your printer IP
        "192.168.1.101",  # Replace with your printer IP
        "192.168.1.102"   # Replace with your printer IP
    )
    
    Write-Host "`nCurrent printer IP list:" -ForegroundColor Yellow
    $printerIPs | ForEach-Object { Write-Host "  $_" }
    Write-Host "Edit the `$printerIPs array in the script to add your printers." -ForegroundColor Gray
    
    $testDoc = "$RANDOM_DIR\printer_test.txt"
    $content = @"
Printer Test Page
Generated: $(Get-Date)
Random ID: $(Get-Random -Minimum 100000 -Maximum 999999)
"@
    $content | Out-File $testDoc
    
    foreach ($ip in $printerIPs) {
        Write-Host "`nTesting printer at $ip ..." -ForegroundColor Cyan
        
        # Test port 9100 (raw printing)
        $portTest = Test-NetConnection $ip -Port 9100 -WarningAction SilentlyContinue -ErrorAction SilentlyContinue
        if ($portTest.TcpTestSucceeded) {
            Write-Host "  Port 9100 OPEN" -ForegroundColor Green
            
            # Try to send a test page
            try {
                if (Test-Command "lpr") {
                    lpr -S $ip -P raw -o l $testDoc 2>$null
                    Write-Host "  Test page sent via lpr" -ForegroundColor Green
                } else {
                    Write-Host "  lpr not available – install Print Services" -ForegroundColor Yellow
                    # Install Print Services feature (requires reboot sometimes)
                    Add-WindowsCapability -Online -Name "Print.Management.Console~~~~0.0.1.0" -ErrorAction SilentlyContinue
                }
                Add-Content $PRINTER_LOG "$(Get-Date) - Sent test page to $ip"
            } catch {
                Write-Host "  Failed to send test page" -ForegroundColor Red
            }
        } else {
            Write-Host "  Port 9100 CLOSED" -ForegroundColor Red
        }
    }
    Write-Log "Printer testing complete. Log saved to $PRINTER_LOG" -Color Green
}

# ========== 10. Anonymous Account Creation ==========
function New-AnonymousAccount {
    Write-Log "Creating anonymous local account..." -Color Cyan
    $anonUsername = "User_" + -join ((65..90) + (97..122) | Get-Random -Count 8 | % {[char]$_})
    $anonPassword = -join ((48..57) + (65..90) + (97..122) | Get-Random -Count 16 | % {[char]$_})
    $securePassword = ConvertTo-SecureString $anonPassword -AsPlainText -Force
    
    try {
        New-LocalUser -Name $anonUsername -Password $securePassword -FullName "Anonymous User" -Description "Auto-generated" -PasswordNeverExpires
        Add-LocalGroupMember -Group "Administrators" -Member $anonUsername
        $credFile = "$OUTPUT_DIR\anon_credentials.txt"
        "Username: $anonUsername`r`nPassword: $anonPassword`r`nCreated: $(Get-Date)" | Out-File $credFile
        Write-Log "Anonymous account created. Credentials saved to: $credFile" -Color Green
        Write-Log "PASSWORD: $anonPassword (save this!)" -Color Yellow
    } catch {
        Write-Log "Failed to create local user: $_" -Color Red
    }
}

# ========== 11. Scheduled Task for Random Content ==========
function New-RandomContentTask {
    $taskName = "MRROBOT_RandomContent"
    $scriptPath = "$env:USERPROFILE\Desktop\GenerateRandomContent.ps1"
    
    $scriptContent = @'
$OUTPUT_DIR = "$env:USERPROFILE\Desktop\RandomContent_$(Get-Date -Format 'yyyyMMdd')"
New-Item -ItemType Directory -Force -Path $OUTPUT_DIR | Out-Null
1..5 | ForEach-Object {
    $fileName = "$OUTPUT_DIR\random_$(Get-Random).txt"
    "Random Data $_`n$(Get-Date)" | Out-File $fileName
}
'@
    $scriptContent | Out-File $scriptPath
    
    try {
        $action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-NoProfile -File `"$scriptPath`""
        $trigger = New-ScheduledTaskTrigger -Daily -At 3am
        $principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest
        Register-ScheduledTask -TaskName $taskName -Action $action -Trigger $trigger -Principal $principal -Force
        Write-Log "Scheduled task '$taskName' created (runs daily at 3am)" -Color Green
    } catch {
        Write-Log "Failed to create scheduled task: $_" -Color Red
    }
}

# ========== 12. Bitcoin Mining Guide ==========
function Show-MiningGuide {
    $miningInfo = @"
                    BITCOIN MINING GUIDE 2026

BTC Price: $73,810.93
Network Hashrate: 1,056 EH/s
Block Reward: 3.125                        Write-Log "Creating anonymous local account..." -Color Cyan
    $anonUsername = "User_" + -join ((65..90) + (97..122) | Get-Random -Count 8 | % {[char]$_})
    $anonPassword = -join ((48..57) + (65..90) + (97..122) | Get-Random -Count 16 | % {[char]$_})
    $securePassword = ConvertTo-SecureString $anonPassword -AsPlainText -Force
    
    try {
        New-LocalUser -Name $anonUsername -Password $securePassword -FullName "Anonymous User" -Description "Auto-generated" -PasswordNeverExpires
        Add-LocalGroupMember -Group "Administrators" -Member $anonUsername
        $credFile = "$OUTPUT_DIR\anon_credentials.txt"
        "Username: $anonUsername`r`nPassword: $anonPassword`r`nCreated: $(Get-Date)" | Out-File $credFile
        Write-Log "Anonymous account created. Credentials saved to: $credFile" -Color Green
        Write-Log "PASSWORD: $anonPassword (save this!)" -Color Yellow
    } catch {
        Write-Log "Failed to create local user: $_" -Color Red
    }
}

# ========== 11. Scheduled Task for Random Content ==========
function New-RandomContentTask {
    $taskName = "MRROBOT_RandomContent"
    $scriptPath = "$env:USERPROFILE\Desktop\GenerateRandomContent.ps1"
    
    $scriptContent = @'
$OUTPUT_DIR = "$env:USERPROFILE\Desktop\RandomContent_$(Get-Date -Format 'yyyyMMdd')"
New-Item -ItemType Directory -Force -Path $OUTPUT_DIR | Out-Null
1..5 | ForEach-Object {
    $fileName = "$OUTPUT_DIR\random_$(Get-Random).txt"
    "Random Data $_`n$(Get-Date)" | Out-File $fileName
}
'@
    $scriptContent | Out-File $scriptPath
    
    try {
        $action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-NoProfile -File `"$scriptPath`""
        $trigger = New-ScheduledTaskTrigger -Daily -At 3am
        $principal = New-ScheduledTaskPrincipal -UserId "SYSTEM" -LogonType ServiceAccount -RunLevel Highest
        Register-ScheduledTask -TaskName $taskName -Action $action -Trigger $trigger -Principal $principal -Force
        Write-Log "Scheduled task '$taskName' created (runs daily at 3am)" -Color Green
    } catch {
        Write-Log "Failed to create scheduled task: $_" -Color Red
    }
}

# ========== 12. Bitcoin Mining Guide ==========
function Show-MiningGuide {
    $miningInfo = @"
                    BITCOIN MINING GUIDE 2026

BTC Price: $73,810.93
Network Hashrate: 1,056 EH/s
Block Reward: 3.125 BTC

MINING HARDWARE COMPARISON:
┌─────────────────────┬──────────┬──────────┬───────────┬────────────┐
│ Miner               │ Hashrate │ Power    │ Daily $   │ Monthly $  │
├─────────────────────┼──────────┼──────────┼───────────┼────────────┤
│ Antminer S21 Hydro  │ 335 TH/s │ 5360 W   │ $2.65     │ $79.50     │
│ WhatsMiner M60      │ 280 TH/s │ 3300 W   │ $2.45     │ $73.50     │
│ Antminer S19 XP     │ 140 TH/s │ 3010 W   │ $0.85     │ $25.50     │
│ Goldshell BOX BTC   │ 1.5 TH/s │ 250 W    │ ($0.45)   │ ($13.50)   │
└─────────────────────┴──────────┴──────────┴───────────┴────────────┘
"@
    $miningInfo | Out-File "$OUTPUT_DIR\mining_guide.txt"
    Write-Host $miningInfo -ForegroundColor Cyan
}

# ========== MAIN EXECUTION ==========
Clear-Host
Write-Host $JOKER_ART -ForegroundColor Green
Write-Host $MEWTWO_ART -ForegroundColor Magenta
Write-Host $CHARIZARD_ART -ForegroundColor Red
Write-Host "`n               M R .   R O B O T      U L T I M A T E   E D I T I O N" -ForegroundColor Cyan
Write-Host "                         Privacy + Pentest + OSINT + Printer" -ForegroundColor Yellow
Write-Host "`nContact: $CONTACT_EMAIL | $CONTACT_PHONE" -ForegroundColor White
Write-Host "Address: $CONTACT_ADDRESS $CONTACT_SINCE" -ForegroundColor White
Write-Host "`nOSINT Targets Loaded: Joseph McMoneagle | Vera Farmiga" -ForegroundColor Magenta

Write-Log "====== MR. ROBOT ULTIMATE STARTING ======" -Color Magenta

# Step 1: Random Content Generation
Write-Log "Step 1: Generating random test data..." -Color Cyan
New-RandomContent -FileCount 10

# Step 2: Bitcoin Wallet
Write-Log "Step 2: Generating Bitcoin wallet..." -Color Cyan
Generate-BitcoinWallet

# Step 3: Tor Service
Write-Log "Step 3: Installing Tor anonymization service..." -Color Cyan
Install-TorService

# Step 4: OSINT Reports
Write-Log "Step 4: Saving OSINT target data..." -Color Cyan
Save-OSINTReports

# Step 5: GitHub Cloning (optional with FIXED Git)
$cloneChoice = Read-Host "`nDo you want to clone all GitHub repositories? (y/N)"
if ($cloneChoice -eq 'y') {
    Clone-AllRepos
} else {
    Write-Log "Skipping GitHub cloning." -Color Yellow
}

# Step 6: Network Discovery
Write-Log "Step 6: Discovering network devices..." -Color Cyan
$devices = Invoke-NetworkDiscovery

# Step 7: Printer Testing
Write-Log "Step 7: Testing network printers..." -Color Cyan
Test-NetworkPrinters

# Step 8: Anonymous Account
Write-Log "Step 8: Creating anonymous local account..." -Color Cyan
New-AnonymousAccount

# Step 9: Scheduled Task
Write-Log "Step 9: Creating scheduled task for random content..." -Color Cyan
New-RandomContentTask

# Step 10: Mining Guide
Write-Log "Step 10: Displaying mining guide..." -Color Cyan
Show-MiningGuide

# ========== Final Summary ==========
$summary = @"
                M R .   R O B O T      U L T I M A T E   E D I T I O N
                          FINAL REPORT

Scan Time: $(Get-Date)
Hosts Discovered: $($devices.Count)

Device List:
$($devices | Format-Table | Out-String)

=====================================================================
CONTACT INFORMATION:
Email: $CONTACT_EMAIL
Phone: $CONTACT_PHONE
Address: $CONTACT_ADDRESS
$CONTACT_SINCE
=====================================================================

OSINT TARGETS:
- Joseph McMoneagle (saved to $OSINT_DIR\joseph_mcmoneagle.txt)
- Vera Farmiga (saved to $OSINT_DIR\vera_farmiga.txt)

BITCOIN WALLET:
Generated in: $BTC_DIR

RANDOM CONTENT:
Generated in: $RANDOM_DIR

GITHUB TOOLS:
Cloned to: $TOOLS_DIR
(if selected)

PRINTER LOG:
$PRINTER_LOG

RESULTS LOCATIONS:
  Main folder: $OUTPUT_DIR
  Network scans: $SCANS_DIR
  System logs: $LOGS_DIR

Scheduled task 'MRROBOT_RandomContent' created (daily at 3am)
"@
$summary | Out-File $SUMMARY_TXT
Write-Host $summary -ForegroundColor Cyan

# Open results folder
Invoke-Item $OUTPUT_DIR

Write-Log "====== MR. ROBOT ULTIMATE COMPLETE ======" -Color Magenta
Write-Host $JOKER_ART -ForegroundColor Green
Write-Host $MEWTWO_ART -ForegroundColor Magenta
Write-Host $CHARIZARD_ART -ForegroundColor Red
Write-Host "M4L4K1MU3RT3 got your ass Bitch" -ForegroundColor Red
Read-Host "`nPress Enter to exit"
✅ What's Fixed
