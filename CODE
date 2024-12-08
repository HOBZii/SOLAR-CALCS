import React, { useState } from 'react';
import { Card, CardHeader, CardTitle, CardContent, CardFooter } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from '@/components/ui/select';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import { RadioGroup, RadioGroupItem } from '@/components/ui/radio-group';

// Translations
const translations = {
  en: {
    title: 'Solar System Calculator',
    systemType: 'System Type',
    onGrid: 'On-Grid',
    hybrid: 'Hybrid',
    pumpingSystem: 'Pumping System',
    language: 'Language',
    loadRequirements: 'Load Requirements',
    loadType: 'Load Type',
    kw: 'Kilowatts (KW)',
    ampere: 'Ampere (A)',
    horsePower: 'Horsepower (HP)',
    backupTime: 'Backup Time (Hours)',
    calculateBtn: 'Calculate',
    results: 'Results',
    modules: 'Number of Modules',
    inverterSize: 'Inverter Size (KW)',
    batteryCapacity: 'Battery Capacity (AH)',
    loadTypeError: 'Please enter load requirements',
    batteryVoltage: '51.2V DC (Standard)',
    panelWattage: '585W (Standard)',
    conversionNote: 'Note: Conversions are based on standard electrical conversions'
  },
  ar: {
    title: 'حاسبة أنظمة الطاقة الشمسية',
    systemType: 'نوع النظام',
    onGrid: 'متصل بالشبكة',
    hybrid: 'هجين',
    pumpingSystem: 'نظام الضخ',
    language: 'اللغة',
    loadRequirements: 'متطلبات الحمل',
    loadType: 'نوع الحمل',
    kw: 'كيلووات (KW)',
    ampere: 'أمبير (A)',
    horsePower: 'حصان (HP)',
    backupTime: 'وقت الاحتياطي (ساعات)',
    calculateBtn: 'احسب',
    results: 'النتائج',
    modules: 'عدد الوحدات',
    inverterSize: 'حجم المحول (كيلووات)',
    batteryCapacity: 'سعة البطارية (أمبير ساعة)',
    loadTypeError: 'يرجى إدخال متطلبات الحمل',
    batteryVoltage: '51.2 فولت تيار مستمر (قياسي)',
    panelWattage: '585 واط (قياسي)',
    conversionNote: 'ملاحظة: التحويلات مبنية على التحويلات الكهربائية القياسية'
  }
};

const SolarCalculator = () => {
  // State variables
  const [language, setLanguage] = useState('en');
  const [systemType, setSystemType] = useState('hybrid');
  const [loadType, setLoadType] = useState('kw');
  const [loadValue, setLoadValue] = useState('');
  const [backupTime, setBackupTime] = useState('');
  const [results, setResults] = useState(null);

  // Constant values
  const PANEL_WATTAGE = 585;
  const BATTERY_VOLTAGE = 51.2;

  // Calculation logic
  const calculateSolarSystem = () => {
    // Input validation
    if (!loadValue) {
      alert(translations[language].loadTypeError);
      return;
    }

    // Convert load to watts based on selected type
    let loadWatts;
    switch(loadType) {
      case 'kw':
        loadWatts = parseFloat(loadValue) * 1000;
        break;
      case 'ampere':
        loadWatts = parseFloat(loadValue) * BATTERY_VOLTAGE;
        break;
      case 'horsePower':
        // 1 HP = 745.7 watts
        loadWatts = parseFloat(loadValue) * 745.7;
        break;
      default:
        loadWatts = 0;
    }

    // System efficiency (typical range 0.7-0.8)
    const systemEfficiency = 0.75;

    // Total daily energy requirement
    const dailyEnergyWh = loadWatts * parseFloat(backupTime);

    // Calculate number of modules
    // Assuming average 5-6 peak sun hours per day
    const peakSunHours = 5.5;
    const totalModulesRequired = Math.ceil(
      (dailyEnergyWh / (PANEL_WATTAGE * peakSunHours * systemEfficiency))
    );

    // Calculate inverter size
    // Typically add 20% overhead to the total load
    const inverterSizeKW = Math.ceil(loadWatts / 1000 * 1.2);

    // Calculate battery capacity
    // Depth of discharge typically 50% for lead-acid, 80% for lithium
    const depthOfDischarge = 0.5; // Conservative estimate
    const batteryCapacityAH = Math.ceil(
      (dailyEnergyWh / BATTERY_VOLTAGE) / depthOfDischarge
    );

    // Set results
    setResults({
      modules: totalModulesRequired,
      inverterSize: inverterSizeKW,
      batteryCapacity: batteryCapacityAH
    });
  };

  // Current translations
  const t = translations[language];

  return (
    <div className={`p-4 ${language === 'ar' ? 'text-right' : 'text-left'}`} 
         dir={language === 'ar' ? 'rtl' : 'ltr'}>
      <Card className="max-w-xl mx-auto">
        <CardHeader>
          <CardTitle>{t.title}</CardTitle>
        </CardHeader>
        <CardContent>
          {/* Language Selection */}
          <div className="mb-4">
            <Label>{t.language}</Label>
            <Select 
              value={language} 
              onValueChange={(value) => setLanguage(value)}
            >
              <SelectTrigger>
                <SelectValue placeholder={t.language} />
              </SelectTrigger>
              <SelectContent>
                <SelectItem value="en">English</SelectItem>
                <SelectItem value="ar">العربية</SelectItem>
              </SelectContent>
            </Select>
          </div>

          {/* System Type */}
          <div className="mb-4">
            <Label>{t.systemType}</Label>
            <Select 
              value={systemType} 
              onValueChange={(value) => setSystemType(value)}
            >
              <SelectTrigger>
                <SelectValue placeholder={t.systemType} />
              </SelectTrigger>
              <SelectContent>
                <SelectItem value="onGrid">{t.onGrid}</SelectItem>
                <SelectItem value="hybrid">{t.hybrid}</SelectItem>
                <SelectItem value="pumpingSystem">{t.pumpingSystem}</SelectItem>
              </SelectContent>
            </Select>
          </div>

          {/* Fixed Panel Wattage */}
          <div className="mb-4">
            <Label>{t.panelWattage}</Label>
            <Input 
              type="text" 
              value={PANEL_WATTAGE + 'W'}
              disabled
            />
          </div>

          {/* Fixed Battery Voltage */}
          <div className="mb-4">
            <Label>{t.batteryVoltage}</Label>
            <Input 
              type="text" 
              value={`${BATTERY_VOLTAGE} VDC`}
              disabled
            />
          </div>

          {/* Load Requirements */}
          <div className="mb-4">
            <Label>{t.loadRequirements}</Label>
            <RadioGroup 
              className="flex space-x-4 mb-2"
              value={loadType} 
              onValueChange={setLoadType}
            >
              <div className="flex items-center space-x-2">
                <RadioGroupItem value="kw" id="kw" />
                <Label htmlFor="kw">{t.kw}</Label>
              </div>
              <div className="flex items-center space-x-2">
                <RadioGroupItem value="ampere" id="ampere" />
                <Label htmlFor="ampere">{t.ampere}</Label>
              </div>
              <div className="flex items-center space-x-2">
                <RadioGroupItem value="horsePower" id="horsePower" />
                <Label htmlFor="horsePower">{t.horsePower}</Label>
              </div>
            </RadioGroup>
            <Input 
              type="number" 
              value={loadValue}
              onChange={(e) => setLoadValue(e.target.value)}
              placeholder={
                loadType === 'kw' ? t.kw : 
                loadType === 'ampere' ? t.ampere : 
                t.horsePower
              }
            />
            <small className="text-muted-foreground">{t.conversionNote}</small>
          </div>

          {/* Backup Time */}
          <div className="mb-4">
            <Label>{t.backupTime}</Label>
            <Input 
              type="number" 
              value={backupTime}
              onChange={(e) => setBackupTime(e.target.value)}
              placeholder={t.backupTime}
            />
          </div>

          {/* Calculate Button */}
          <Button onClick={calculateSolarSystem} className="w-full">
            {t.calculateBtn}
          </Button>

          {/* Results */}
          {results && (
            <Card className="mt-4">
              <CardHeader>
                <CardTitle>{t.results}</CardTitle>
              </CardHeader>
              <CardContent>
                <p>{t.modules}: {results.modules}</p>
                <p>{t.inverterSize}: {results.inverterSize} KW</p>
                <p>{t.batteryCapacity}: {results.batteryCapacity} AH</p>
              </CardContent>
            </Card>
          )}
        </CardContent>
      </Card>
    </div>
  );
};

export default SolarCalculator;

Version 5 of 5



