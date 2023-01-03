# -*- coding: utf-8 -*-

# Veri Tabani Bakim Islem Adimlari ---TEST VT---
# 2022- Yunus Emre Onder

import arcpy, time

sdeFolder = "SDEFOLDERPATH"
sdeNames = ['TEST1',
            'TEST2']
logFile = "C:/Users/24987/Desktop/reconcilelog.txt"
print "islemler basliyor..."

for sde in sdeNames:
    sdePath = sdeFolder + '\\' + sde  # sde yolu
    owner = sde.split(".")[0].split("_")[-1]
    arcpy.env.workspace = sdePath
    workspace = arcpy.env.workspace
    try:
        arcpy.AcceptConnections(sdePath, False) # connectionlari kill eder
        print owner + " icin yeni baglantilar engellendi"
        # time.sleep(900) # 15 dakika bekler
        arcpy.DisconnectUser(sdePath, "ALL")
        print owner + " icin tum kullanicilar kill edildi"
        versionList = arcpy.ListVersions(sdePath)
        # arcpy.ReconcileVersions_management(sdePath, "ALL_VERSIONS", owner + ".DEFAULT", versionList, "LOCK_ACQUIRED", # Reconcile islemini yapar
        #                                       "NO_ABORT", "BY_OBJECT", "FAVOR_TARGET_VERSION", "POST", "KEEP_VERSION", logFile)
        arcpy.Compress_management(sdePath) # compress islemini yapar
    except Exception as err:
        print owner + " connection kill vb islemi tamamlanamadi!!!"
        print err
    pass
    arcpy.AcceptConnections(sdePath, True) # connectionlari kabul eder
    print owner + " icin yeni baglanti kabul edilebilir hale getirildi"

    dataList = arcpy.ListTables(owner + '.*') + arcpy.ListFeatureClasses(owner + '.*')
    for dataset in arcpy.ListDatasets(owner + '.*'):
        dataList += arcpy.ListFeatureClasses(feature_dataset=dataset)
    try:
        print "Rebuild Indexes islemi basladi"
        arcpy.RebuildIndexes_management(workspace, "NO_SYSTEM", dataList, "ONLY_DELTAS") # rebuildIndexes islemini yapar
        print "Rebuild Indexes Success" + owner
    except Exception as err:
        print owner + " Rebuild Indexes islemi basarisiz oldu"
        pass
    try:
        print "Analyze Dataset islemi basladi"
        arcpy.AnalyzeDatasets_management(workspace, "NO_SYSTEM", "ANALYZE_BASE", "ANALYZE_DELTA", "ANALYZE_ARCHIVE") # AnalyzeDataset islemini yapar
        print "Analyze Dataseti islemi basarili sonuclandi" + owner
    except Exception as err:
        print owner + " Analyze Dataset islemi basarisiz oldu"
        pass
print "islemler tamamlandi"
