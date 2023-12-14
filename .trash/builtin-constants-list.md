# Math

> ##### `add e1 e2` ^add
> ##### `sub e1 e2` ^sub
> ##### `mul e1 e2` ^mul
> ##### `div e1 e2` ^div

> ##### `bitAnd e1 e2` ^bit-and
> ##### `bitOr e1 e2` ^bit-or
> ##### `bitXor e1 e2` ^bit-xor
> ##### `ceil double` ^ceil
> ##### `floor double` ^floor

# Core

> ##### `typeOf e` ^typeOf
> ##### `abort s` ^abort
> ##### `throw s` ^throw
> ##### `break v` ^break
> ##### `import path` ^import
> ##### `derivation attrs` ^derivation

# Checks

> ##### `all pred list` ^all
> ##### `any pred list` ^any
> ##### `isAttrs e` ^isAttrs
> ##### `isBool e` ^isBool
> ##### `isFloat e` ^isFloat
> ##### `isFunction e` ^isFunction
> ##### `isInt e` ^isInt
> ##### `isList e` ^isList
> ##### `isNull e` ^isNull
> ##### `isPath e` ^isPath
> ##### `isString e` ^isString

# Conversions

> ##### `toFile name s` ^toFile
> ##### `toJSON e` ^toJSON
> ##### `toPath s` ^toPath
> ##### `toString e` ^toString
> ##### `toXML e` ^toXML

# File system

> ##### `path args` ^path
> ##### `pathExists path` ^pathExists
> ##### `dirOf s` ^dirOf

# List

> ##### `elem x xs` ^elem
> ##### `elemAt xs n` ^elemAt

# Unsorted

> ##### `addDrvOutputDependencies s` ^addDrvOutputDependencies
> ##### `attrNames set` ^attrNames
> ##### `attrValues set` ^attrValues
> ##### `baseNameOf s` ^baseNameOf
> ##### `catAttrs attr list` ^catAttrs
> ##### `compareVersions s1 s2` ^compareVersions
> ##### `concatLists lists` ^concatLists
> ##### `concatMap f list` ^concatMap
> ##### `concatStringsSep separator list` ^concatStringsSep
> ##### `convertHash args` ^convertHash
> ##### `deepSeq e1 e2` ^deepSeq
> ##### `fetchClosure args` ^fetchClosure
> ##### `fetchGit args` ^fetchGit
> ##### `fetchTarball args` ^fetchTarball
> ##### `fetchurl url` ^fetchurl
> ##### `filter f list` ^filter
> ##### `filterSource e1 e2` ^filterSource
> ##### `findFile search-path lookup-path` ^findFile
> ##### `flakeRefToString attrs` ^flakeRefToString
> ##### `foldl' op nul list` ^foldl'
> ##### `fromJSON e` ^fromJSON
> ##### `fromTOML e` ^fromTOML
> ##### `functionArgs f` ^functionArgs
> ##### `genList generator length` ^genList
> ##### `genericClosure attrset` ^genericClosure
> ##### `getAttr s set` ^getAttr
> ##### `getContext s` ^getContext
> ##### `getEnv s` ^getEnv
> ##### `getFlake args` ^getFlake
> ##### `groupBy f list` ^groupBy
> ##### `hasAttr s set` ^hasAttr
> ##### `hasContext s` ^hasContext
> ##### `hashFile type p` ^hashFile
> ##### `hashString type s` ^hashString
> ##### `head list` ^head
> ##### `intersectAttrs e1 e2` ^intersectAttrs
> ##### `length e` ^length
> ##### `lessThan e1 e2` ^lessThan
> ##### `listToAttrs e` ^listToAttrs
> ##### `map f list` ^map
> ##### `mapAttrs f attrset` ^mapAttrs
> ##### `match regex str` ^match
> ##### `outputOf derivation-reference output-name` ^outputOf
> ##### `parseDrvName s` ^parseDrvName
> ##### `parseFlakeRef flake-ref` ^parseFlakeRef
> ##### `partition pred list` ^partition
> ##### `placeholder output` ^placeholder
> ##### `readDir path` ^readDir
> ##### `readFile path` ^readFile
> ##### `readFileType p` ^readFileType
> ##### `removeAttrs set list` ^removeAttrs
> ##### `replaceStrings from to s` ^replaceStrings
> ##### `seq e1 e2` ^seq
> ##### `sort comparator list` ^sort
> ##### `split regex str` ^split
> ##### `splitVersion s` ^splitVersion
> ##### `storePath path` ^storePath
> ##### `stringLength e` ^stringLength
> ##### `substring start len s` ^substring
> ##### `tail list` ^tail
> ##### `trace e1 e2` ^trace
> ##### `traceVerbose e1 e2` ^traceVerbose
> ##### `tryEval e` ^tryEval
> ##### `unsafeDiscardOutputDependency s` ^unsafeDiscardOutputDependency
> ##### `zipAttrsWith f list` ^zipAttrsWith