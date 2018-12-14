project('ogon-greeter-qt', 'cpp',
		version: '1.0.0',
		default_options: [
			'warning_level=2',
			'c_std=gnu99',
			'b_lundef=false',
		],
		meson_version: '>= 0.47',
		license: 'GNU/AGPL',
	)
	
deps_ogon = dependency('ogon1')
protocols_dir = deps_ogon.get_pkgconfig_variable('protocol_dir')

protobuf_compiler = find_program('protoc')

generated_sbp_proto = custom_target('gen-sbp',
	input: [join_paths(protocols_dir, 'protobuf', 'SBP.proto')],
	output: ['SBP.pb.cc', 'SBP.pb.h'],
	command: [protobuf_compiler, '--proto_path', join_paths(protocols_dir, 'protobuf'), '--cpp_out', '@OUTDIR@', '@INPUT@'],
)

qt5 = import('qt5')
moc_files = qt5.preprocess(
	moc_headers: ['abstract_greeter.h', 'lowres_greeter.h', 'nice_greeter.h'],
	qresources: 'ogon_qt_greeter.qrc',
	ui_files: ['forms/nice_greeter.ui', 'forms/small_greeter.ui']
)

deps_qt5 = dependency('qt5', modules: ['Core', 'Gui', 'Widgets'], private_headers: true)
deps_qogon = dependency('qogon')
deps_protobuf = dependency('protobuf')

add_global_arguments([ 
	'-DTRANSLATIONS_DIR="@0@"'.format(translations_dir)
],	language: 'cpp',
)
greeter = executable('ogon-qt-greeter',
	sources: [
		'abstract_greeter.cpp',
		'lowres_greeter.cpp',
		'nice_greeter.cpp',
		'main.cpp',
		generated_sbp_proto,
		moc_files
	],
	dependencies: [deps_qt5, deps_qogon, deps_protobuf],
	install: true,
)

# i18n translations
translations_dir = join_paths(get_option('prefix'), 'share', 'qt5', 'translations')
qt5.compile_translations(
	ts_files: ['greeter_de.ts', 'greeter_fr.ts', 'greeter_en.ts',],
	install_dir: translations_dir,
	install: true
) 
